---
layout: page
---

This page is used for taking my notes during my daily programming works. It helps myself, of course, on quick references before searching. And I am glad if you find it useful.

## **ASP.NET MVC**

*   #### **Ignore a path for downloads** (or accessing static files in ASP.NET MVC Web Application)
    Add a line below to the `RegisterRoutes()` method in the `RouteConfig.cs` file.
    
    ```csharp
    routes.IgnoreRoute("Downloads/*");
    ```
  
    or to ignore all
    ```csharp
    routes.IgnoreRoute("");
    ```

    Full class
    ```csharp
    public class RouteConfig
    {
        public static void RegisterRoutes(RouteCollection routes)
        {
            routes.IgnoreRoute("{resource}.axd/{*pathInfo}");

            // allow to access static files under /Downloads/*.* = no routing this path to Controller in MVC
            routes.IgnoreRoute("Downloads/*");
            //routes.IgnoreRoute("");

            routes.MapRoute(
                name: "Default",
                url: "{controller}/{action}/{id}",
                defaults: new { controller = "Home", action = "Index", id = UrlParameter.Optional }
            );
        }
    }
    ```

## **Front-End Libraries (CSS, HTML, JavaScript, jQuery plugins...)**

*   #### **SweetAlert**: makes popup messages pretty.
    Link: <https://sweetalert.js.org/>

*   #### **Owl Carousel 2**: creates a beautiful responsive slider, touch enabled
    Link: <https://owlcarousel2.github.io/OwlCarousel2/>

*   #### **VenoBox**: responsive jQuery Lightbox plugin, suitable for images, inline contents, iFrames, Google-Maps, Ajax requests, Vimeo and YouTube videos
    Link: <https://veno.es/venobox/>

*   #### **Isotope**: position items with different layout modes, sorting, filters...
    Link: <https://isotope.metafizzy.co/>

*   #### **AOS**: Animate On Scroll Library
    Link: <https://michalsnik.github.io/aos/>

*   #### **GLightbox**: A touchable Pure Javascript lightbox with mobile and video support.
    Link: <https://biati-digital.github.io/glightbox/>

*   #### **Magnific Popup**: a responsive lightbox & dialog script
    Link: <https://dimsemenov.com/plugins/magnific-popup/>
    
    Example of using **Magnific Popup** for playing YouTube is [here](/asset/html/magnific-popup-youtube-player.html)

*   #### **BootstrapMade**: Free Bootstrap Templates
    Link: <https://bootstrapmade.com/>

*   #### **CSS-trick**: overflow-wrap
    Link: <https://css-tricks.com/almanac/properties/o/overflow-wrap/>

    My code example copied from above link is [here](/asset/html/overflow-wrap.html)

    **Remember** to use `overflow-wrap` or `text-break` (preferred) from [Bootstrap](https://getbootstrap.com/docs/5.0/utilities/text/#word-break) to prevent long strings of text from breaking your components' layout

*   #### **cookie-consent-js**: A simple dialog and framework to handle the German and EU law (as written by EuGH, 1.10.2019 – C-673/17) about cookies in a website.
    Link: <https://github.com/shaack/cookie-consent-js>

## **Newtonsoft Json.NET**

*   #### **JToken**
    Link: <https://www.newtonsoft.com/json/help/html/T_Newtonsoft_Json_Linq_JToken.htm>

    The JToken hierarchy looks like this:

    ```
    JToken              - abstract base class     
        JContainer      - abstract base class of JTokens that can contain other JTokens
            JArray      - represents a JSON array (contains an ordered list of JTokens)
            JObject     - represents a JSON object (contains a collection of JProperties)
            JProperty   - represents a JSON property (a name/JToken pair inside a JObject)
        JValue          - represents a primitive JSON value (string, number, boolean, null)
    ```

## **EF & EF Core**

*   #### **Differences** in connection string

    **2 flashes** `\\` in connection string `Server=(LocalDB)\\MSSQLLocalDB` in `appsettings.json` file
    ```json
    {
        "ConnectionStrings": 
        {
            "DefaultConnection": "Server=(LocalDB)\\MSSQLLocalDB;Database=AppDb;Trusted_Connection=True;"
        }
    }
    ```

    but **ONLY 1 flash** `\` in connection string `Server=(LocalDB)\MSSQLLocalDB` in `dotnet ef dbcontext scaffold` command

    ```powershell
    dotnet ef dbcontext scaffold "Server=(LocalDB)\MSSQLLocalDB;Database=AppDb;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -o Models
    ```
