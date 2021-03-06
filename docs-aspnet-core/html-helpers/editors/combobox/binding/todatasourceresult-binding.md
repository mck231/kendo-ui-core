---
title: Custom Data Binding
page_title: Custom Data Binding | Kendo UI ComboBox HtmlHelper for ASP.NET Core
description: "Learn how to implement custom ToDataSourceResult data binding in the Kendo UI ComboBox HtmlHelper for ASP.NET Core (MVC 6 or ASP.NET Core MVC)."
slug: htmlhelpers_combobox_todatasourceresultbinding_aspnetcore
position: 4
---

# Custom Data Binding

Below are listed the steps for you to follow when configuring the Kendo UI ComboBox to use a custom DataSource and thus bind to a `ToDataSourceResult` instance.

1. Create an action method which renders the view.

    ###### Example

        public IActionResult Index()
        {
            return View();
        }

1. Create a new action method and pass the **Products** table as JSON result.

    ###### Example

        public JsonResult GetProducts([DataSourceRequest] DataSourceRequest request)
        {
            NorthwindDataContext northwind = new NorthwindDataContext();

            return Json(northwind.Products.ToDataSourceResult(request));
        }

1. Add an Ajax-bound ComboBox.

    ###### Example

        @(Html.Kendo().ComboBox()
            .Name("productComboBox") //The name of the ComboBox is mandatory. It specifies the "id" attribute of the widget.
            .DataTextField("ProductName") //Specify which property of the Product to be used by the ComboBox as a text.
            .DataValueField("ProductID") //Specify which property of the Product to be used by the ComboBox as a value.
            .DataSource(source =>
            {
                source.Custom()
                        .ServerFiltering(true)
                        .Type("aspnetmvc-ajax") //Set this type if you want to use DataSourceRequest and ToDataSourceResult instances.
                        .Transport(transport =>
                        {
                            transport.Read("GetProducts", "Home");
                        })
                        .Schema(schema =>
                        {
                            schema.Data("Data") //define the [data](http://docs.telerik.com/kendo-ui/api/javascript/data/datasource#configuration-schema.data) option
                                .Total("Total"); //define the [total](http://docs.telerik.com/kendo-ui/api/javascript/data/datasource#configuration-schema.total) option
                        });
            })
        )

## See Also

* [JavaScript API Reference of the ComboBox](http://docs.telerik.com/kendo-ui/api/javascript/ui/combobox)
* [ComboBox HtmlHelper for ASP.NET MVC](http://docs.telerik.com/aspnet-mvc/helpers/combobox/overview)
* [ComboBox Official Demos](http://demos.telerik.com/aspnet-core/combobox/index)
