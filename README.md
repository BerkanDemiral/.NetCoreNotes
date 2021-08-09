# .NetCoreNotes


## Ders1:

- Açık kaynaklı ve cross platform özelliğine sahiptir. Microsoft tarafından geliştirilmiştir.
- Client-Side Freamwork ile (angularjs, reactjs..) tam bir uyum içerisindedir. İçerisinde bu yapılar da kullanılabilir.
- Host etmek oldukça kolaydır.
- VSCode, VSStudio ile geliştirilebilir.

---

---

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/84f462b9-6beb-4b6c-a26e-e3b8096766af/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/84f462b9-6beb-4b6c-a26e-e3b8096766af/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d92f90e3-c340-4ee0-b5f8-422f78539e55/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d92f90e3-c340-4ee0-b5f8-422f78539e55/Untitled.png)

- Burada hem MVC Kullanarak, API kullanarak veya boş bir proje oluşturarak başlayabiliriz ama diğerleri daha ileri seviye oldukları için başlangıçta boş bir proje oluşturulması daha uygundur.

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b3a494b4-9135-449d-a908-0ce7f00cc315/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b3a494b4-9135-449d-a908-0ce7f00cc315/Untitled.png)

- **startup.cs** içerisinde c# uzantısına sahip olarak programımızın ilk çalışacağı kodlar bulunmaktadır. Burada [**app.run**](http://app.run) kısmındaki "Hello World" değeri sayesinde başlangıçta ekrana Hello World yazısı yazdırılmaktadır.

---

---

### Kodlamaya ve Route Yapısına Giriş:

- Programın taslağını oluştururken MVC'deki klasörleme yapısından yararlanırız. Bir controller oluşturur ve frontend tarafındaki view çalıştırıldığı zaman içindeki kodların çalışacağı ActionResult(tetikleyici) yapılarından oluşan bir iskelet kurarız.

    ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/dc127349-45fd-449c-b67e-9796a530fbd9/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/dc127349-45fd-449c-b67e-9796a530fbd9/Untitled.png)

- Ama Contoller ve View yapılarını başlangıçta oluştursak da program yapısı gereği startup.cs dosyasından başlar ve bizim kodlarımız çalışmaz. Bunun önlenebilmesi için startup.cs içerisinde bizim klasörleme mantığımıza uyan bir route yapısı oluşturumalı ve programımızın başlangıç kısmını vermemiz gerekmektedir.

```jsx
public void ConfigureServices(IServiceCollection services)
        {
            services.AddMvc();
        }
```

startup.cs içerisinde ConfigureServices içerisinde bir AddMvc() servisi eklemeliyiz. 

- Ardından bu Mvc servisinin route yapısını oluşturmalıyız.

    ```jsx
    if (env.IsDevelopment())
                {
                    app.UseDeveloperExceptionPage();
                }

                app.UseMvc(routes =>
                {
                    routes.MapRoute(
                            name: "default",
                            template: "{contoller:home}/{actions:Index}"
                            );
                });
    ```

    - Burada arrow functions kullanarak bir route yapısı oluşturulmuştur. Program çalıştığında direkt olarak return edilmektedir. name kısmı default, template kısmında ise program çalıştığında hangi controller çalışsın ve bu controllerin içindeki hangi tetikleyici(ActionResult) çalışsın gibi bilgileri girmiş bulunmaktayız.

    ### Razor Syntax ve @foreach Yapısı:

    - Razor Syntax: Frontend olarak adlandırılabilecek view tarafında C# kodları yazmamıza olanak sağlayan kısımdır. Örneğin react yazarken jsx dosyaları içerisinde javascript kodları yazabilmek için {{ }} parantezleri kullanılır. Bu da mantık olarak bir razor syntax yapısıdır.

    ```jsx
    <body>
        <div>
            DateTime.Now.ToShortDateString(); @*Direkt string olarak bu çıktıyı verir*@
        </div>

        <br />

        <div>
            @DateTime.Now.ToShortDateString(); @*Bugünün Tarihini yazdırır.*@
        </div>
    </body>
    ```

    ```jsx
    <div>
            @{ 
                var list = new List<string>() { "İstanbul", "Ankara", "Adana", "Burssa",
     "Tekirdağ" };
            }
            @foreach(var i in list)
            {
                @i <br />
            }
        </div>
    ```

    - Aslında controller tarafı normal bir c# dosyasını kapsamaktadır. Bu edenle IActionResult fonksiyonu yerine normal fonksiyonlar da yazılabilmektedir.

        Dilersek int dilersek string formatında fonksiyonlar yazarak bunları döndürebiliriz. 

        ```jsx
        public int Index3()
                {
                    return 10;
                }
        ```

        ---

        ---

    ### Model ve ViewBag Yapıları:

    - Model kısmında veritabanı dosyalarımızı tutabilmekteyiz.

        !! Eğer code first mantığı ile proplar oluşturmak istiyorsak. **prop + tab + tab** diyerek propumuzu otomatik olarak oluşturmaktayız. 

        ```jsx
        namespace First_Project.Models
        {
            public class Book
            {
                public int Id { get; set; }
                public int BookName { get; set; }
                public int Author { get; set; }
            }
        }
        ```

        - Yukarıdaki kodda Models klasörü içerisinde bir book class'ı oluşturduk ve bu book classına ait propertielerin tanımlamalarını ve kapsülleme işlemlerini gerçekleştirdik.
        - Ardından bu yapının frontend kısmına yansıtılması ve kullanılabilmesi için backend kısmında yani controller yapısı içerisinde tanımlanması gerekmektedir. Bu nedenle

        → ***using First_Project.Models;*** tanımlaması yapılmaktadır. 

        - Bu yapının kullanılacağı ActionResult içerisinde ise gerekli kodlar o class'a veya veritabanına bağlı bir şekilde yazılabilmektedir.

        ```jsx
        public IActionResult Index3()
                {
                    var bk = new List<Book>()
                    {
                        new Book()
                        {
                            Id=1, BookName="Çalıkuşu",Author="Reşat Nuri Güntekin"
                        },
                        new Book()
                        {
                            Id=2, BookName="Fahrenheit 451",Author="Ray Bradbury"
                        }
                    };
                    return View(bk);
                }
        ```

        - Normalde controller içerisinde bu şekilde models katmanına ait veri girişlerinin yapılması yanlıştır. Bu kodlar veritabanı veya models katmanı içerisinde bulunmalı ve belirli yöntemler ile çağırılmalıdırlar.
        - Arından view katmanının bu actionresulta bağlı index kısmında gerekli frontend kodlarını yazarız.

        > Eğer bir view içerisinde bir modele ait kodlar Razor Syntax ile kodlanacaksa:

        - [model List<...className> tanımı yapılmalıdır.](https://www.notion.so/model-List-className-tan-m-yap-lmal-d-r-fab729982fc74fcda78087a79e239c22)
            - @model List<First_Project.Models.Books>

            ```jsx
            <body>
                <h3>Kitap Sayfasına Hoşgelidiniz</h3>
                <br />
                <br />
                <table>
                    @foreach (var b in Model)
                    {
                        <tr>
                            <td>@b.Id</td>
                            <td>@b.BookName</td>
                            <td>@b.Author</td>
                            <br />
                        </tr>
                    }
                </table>
            </body>
            ```
