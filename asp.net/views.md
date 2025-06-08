# Views


## Transfer data across two controllers.

`TempData`


## Layouts

Layouts don't have a model.

**/Views/Shared/_Layout.cshtml**
```html
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />

<title>@ViewData["Title"]</title>

@RenderBody() <!-- Renders the body of the main (Index) View -->
```

**/Views/Index.cshtml**
```html
@model MainProgram.Models.Model;

@{
    Layout = "_Layout";
}

<body>
    <!-- HTML -->
</body>
```

## Partial views

**/Views/Shared/_PartialView.cshtml**
```html
@model MainProgram.Models.Model;

@{
    // some settings
}

<body>
<!-- HTML -->
</body>
```


**/Views/Index.cshtml**
```html
@model MainProgram.Models.Model;

@{
    // some settings
}

<body>
<!-- HTML -->

@await Html.PartialAsync("_PartialView", Model);

<!-- Since the Partial View requires the same model as we have in our view. You put `Model`. If your partial view requires no model. Then you can use: 
-->

@await Html.PartialAsync("_PartialView");

</body>
```