# Interview Question
## Entity Framework ve C# Kullanarak Veritabanı Modelleme
Bu soruda, Entity Framework ve C# kullanarak `User`, `Post` ve `Tag` olmak üzere üç farklı entity oluşturulması ve `Post` ve `Tag` arasında many-to-many ilişkisi kurmak için `PostTag` adlı bir join table'ın kullanılması istenmektedir. Ayrıca `PostTag` join tablosu içinde `Tag`'in `Post`taki önceliğini belirten `Order` adlı bir parametresi bulunmalıdır. Her `User`'ın birçok `Post`'u olabilir ve her `User`'ın davet eden kullanıcısını belirten bir `invitedBy` parametresi olmalıdır.

## Entity Sınıfları
İlk olarak, Entity Framework kullanarak `User`, `Post` ve `Tag` sınıflarını oluşturmalıyız. Ayrıca `PostTag` adlı bir sınıf oluşturarak, `Post` ve `Tag` arasındaki many-to-many ilişkisini tanımlamalıyız. `PostTag` sınıfının içinde ayrıca `Order` adlı bir parametresine tanımlanmalıdır.

*Örnek Modeller*
```c#
public class User
{
    public int Id { get; set; }
    public string Name { get; set; }
    public ICollection<Post> Posts { get; set; }
    public User InvitedBy { get; set; }
}

public class Post
{
    public int Id { get; set; }
    public string Title { get; set; }
    public DateTime PublishDate { get; set; } = DateTime.UtcNow;
    public ICollection<Tag> Tags { get; set; }
    public User User { get; set; }
}

public class Tag
{
    public int Id { get; set; }
    public string Name { get; set; }
    public ICollection<Post> Posts { get; set; }
}

public class PostTag
{
    public int PostId { get; set; }
    public Post Post { get; set; }
    public int TagId { get; set; }
    public Tag Tag { get; set; }
    public int Order { get; set; }
}
```
_Modeller örnektir düzenlenmeleri gerekmektedir._

## Soru
Veritabanı oluşturma ve ilişkilendirme işlemlerini tamamladıktan sonra:
Veritabanındaki tüm kullanıcıları onları davet eden kişinin ismiyle birlikte listeleyen ve her kullanıcının en son attığı postun başlığını ve o posttaki ilk 2 tag'ı altaki örnekte olduğu gibi yazdıracak kodu yazalım.
```markdown
Ahmet Tarık Duyar
    - C de "flexible array member" nasıl kullanılır? [C99, Tutorial]
Zafer Alp Çetin (invited by Ahmet Tarık Duyar)
    - Strategy Design Pattern? [Dart, Pattern]
```
*İlk iki tag `PostTag` tablosundaki `Order` parametresine göre alınmalıdır.
