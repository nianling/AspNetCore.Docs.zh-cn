
## <a name="add-validation-rules-to-the-movie-model"></a><span data-ttu-id="cad1e-101">将验证规则添加到电影模型</span><span class="sxs-lookup"><span data-stu-id="cad1e-101">Add validation rules to the movie model</span></span>

<span data-ttu-id="cad1e-102">打开 Movie.cs 文件  。</span><span class="sxs-lookup"><span data-stu-id="cad1e-102">Open the *Movie.cs* file.</span></span> <span data-ttu-id="cad1e-103">DataAnnotations 命名空间提供一组内置验证特性，可通过声明方式应用于类或属性。</span><span class="sxs-lookup"><span data-stu-id="cad1e-103">The DataAnnotations namespace provides a set of built-in validation attributes that are applied declaratively to a class or property.</span></span> <span data-ttu-id="cad1e-104">DataAnnotations 还包含 `DataType` 等格式特性，有助于格式设置但不提供任何验证。</span><span class="sxs-lookup"><span data-stu-id="cad1e-104">DataAnnotations also contains formatting attributes like `DataType` that help with formatting and don't provide any validation.</span></span>

<span data-ttu-id="cad1e-105">更新 `Movie` 类以使用内置的 `Required`、`StringLength`、`RegularExpression` 和 `Range` 验证特性。</span><span class="sxs-lookup"><span data-stu-id="cad1e-105">Update the `Movie` class to take advantage of the built-in `Required`, `StringLength`, `RegularExpression`, and `Range` validation attributes.</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc//sample/MvcMovie22/Models/MovieDateRatingDA.cs?name=snippet1)]

<span data-ttu-id="cad1e-106">验证特性指定要对应用这些特性的模型属性强制执行的行为：</span><span class="sxs-lookup"><span data-stu-id="cad1e-106">The validation attributes specify behavior that you want to enforce on the model properties they're applied to:</span></span>

* <span data-ttu-id="cad1e-107">`Required` 和 `MinimumLength` 特性表示属性必须有值；但用户可输入空格来满足此验证。</span><span class="sxs-lookup"><span data-stu-id="cad1e-107">The `Required` and `MinimumLength` attributes indicate that a property must have a value; but nothing prevents a user from entering white space to satisfy this validation.</span></span>
* <span data-ttu-id="cad1e-108">`RegularExpression` 特性用于限制可输入的字符。</span><span class="sxs-lookup"><span data-stu-id="cad1e-108">The `RegularExpression` attribute is used to limit what characters can be input.</span></span> <span data-ttu-id="cad1e-109">在上述代码中，即“Genre”（分类）：</span><span class="sxs-lookup"><span data-stu-id="cad1e-109">In the preceding code, "Genre":</span></span>

  * <span data-ttu-id="cad1e-110">只能使用字母。</span><span class="sxs-lookup"><span data-stu-id="cad1e-110">Must only use letters.</span></span>
  * <span data-ttu-id="cad1e-111">第一个字母必须为大写。</span><span class="sxs-lookup"><span data-stu-id="cad1e-111">The first letter is required to be uppercase.</span></span> <span data-ttu-id="cad1e-112">不允许使用空格、数字和特殊字符。</span><span class="sxs-lookup"><span data-stu-id="cad1e-112">White space, numbers, and special characters are not allowed.</span></span>

* <span data-ttu-id="cad1e-113">`RegularExpression`“Rating”（分级）：</span><span class="sxs-lookup"><span data-stu-id="cad1e-113">The `RegularExpression` "Rating":</span></span>

  * <span data-ttu-id="cad1e-114">要求第一个字符为大写字母。</span><span class="sxs-lookup"><span data-stu-id="cad1e-114">Requires that the first character be an uppercase letter.</span></span>
  * <span data-ttu-id="cad1e-115">允许在后续空格中使用特殊字符和数字。</span><span class="sxs-lookup"><span data-stu-id="cad1e-115">Allows special characters and numbers in  subsequent spaces.</span></span> <span data-ttu-id="cad1e-116">“PG-13”对“分级”有效，但对于“分类”无效。</span><span class="sxs-lookup"><span data-stu-id="cad1e-116">"PG-13" is valid for a rating, but fails for a "Genre".</span></span>

* <span data-ttu-id="cad1e-117">`Range` 特性将值限制在指定范围内。</span><span class="sxs-lookup"><span data-stu-id="cad1e-117">The `Range` attribute constrains a value to within a specified range.</span></span>
* <span data-ttu-id="cad1e-118">`StringLength` 特性使你能够设置字符串属性的最大长度，以及可选的最小长度。</span><span class="sxs-lookup"><span data-stu-id="cad1e-118">The `StringLength` attribute lets you set the maximum length of a string property, and optionally its minimum length.</span></span>
* <span data-ttu-id="cad1e-119">从本质上来说，需要值类型（如 `decimal`、`int`、`float`、`DateTime`），但不需要 `[Required]` 特性。</span><span class="sxs-lookup"><span data-stu-id="cad1e-119">Value types (such as `decimal`, `int`, `float`, `DateTime`) are inherently required and don't need the `[Required]` attribute.</span></span>

<span data-ttu-id="cad1e-120">让 ASP.NET Core 强制自动执行验证规则有助于提升你的应用的可靠性。</span><span class="sxs-lookup"><span data-stu-id="cad1e-120">Having validation rules automatically enforced by ASP.NET Core helps make your app more robust.</span></span> <span data-ttu-id="cad1e-121">同时它能确保你无法忘记验证某些内容，并防止你无意中将错误数据导入数据库。</span><span class="sxs-lookup"><span data-stu-id="cad1e-121">It also ensures that you can't forget to validate something and inadvertently let bad data into the database.</span></span>