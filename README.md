# HTML_CSS_Assignment

@model YourNamespace.Models.ProductViewModel

@using (Html.BeginForm())
{
    <div>
        @Html.LabelFor(m => m.SelectedCategoryId, "Select Category")
        @Html.DropDownListFor(m => m.SelectedCategoryId, 
            new SelectList(Model.Categories, "Id", "Name"), 
            "Select a category", 
            new { @class = "form-control", id = "categoryDropdown" })
    </div>

    <div>
        @Html.LabelFor(m => m.SelectedProductId, "Select Product")
        @Html.DropDownListFor(m => m.SelectedProductId, 
            new SelectList(Model.Products, "Id", "Name"), 
            "Select a product", 
            new { @class = "form-control", id = "productDropdown" })
    </div>

    <input type="submit" value="Submit" class="btn btn-primary" />
}

@section Scripts {
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.5.1/jquery.min.js"></script>
    <script type="text/javascript">
        $(document).ready(function () {
            $('#categoryDropdown').change(function () {
                var categoryId = $(this).val();
                if (categoryId) {
                    $.ajax({
                        url: '@Url.Action("GetProductsByCategory", "Product")',
                        type: 'GET',
                        data: { categoryId: categoryId },
                        success: function (data) {
                            var productDropdown = $('#productDropdown');
                            productDropdown.empty();
                            productDropdown.append('<option value="">Select a product</option>');
                            $.each(data, function (i, product) {
                                productDropdown.append($('<option></option>').val(product.Id).html(product.Name));
                            });
                        },
                        error: function (ex) {
                            alert('Failed to retrieve products: ' + ex);
                        }
                    });
                } else {
                    $('#productDropdown').empty();
                    $('#productDropdown').append('<option value="">Select a product</option>');
                }
            });
        });
    </script>
}
