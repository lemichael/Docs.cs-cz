```console
npm run release
```

<span data-ttu-id="b11bd-101">Tento příkaz vypočítá prostředky klienta ke zpracování při spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="b11bd-101">This command yields the client-side assets to be served when running the app.</span></span> <span data-ttu-id="b11bd-102">Prostředky jsou umístěny v *wwwroot* složky.</span><span class="sxs-lookup"><span data-stu-id="b11bd-102">The assets are placed in the *wwwroot* folder.</span></span>

<span data-ttu-id="b11bd-103">Webpack dokončit následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="b11bd-103">Webpack completed the following tasks:</span></span>

* <span data-ttu-id="b11bd-104">Vymazat obsah *wwwroot* adresáře.</span><span class="sxs-lookup"><span data-stu-id="b11bd-104">Purged the contents of the *wwwroot* directory.</span></span>
* <span data-ttu-id="b11bd-105">Převést TypeScript na JavaScript&mdash;tento proces se označuje jako *transpilation*.</span><span class="sxs-lookup"><span data-stu-id="b11bd-105">Converted the TypeScript to JavaScript&mdash;a process known as *transpilation*.</span></span>
* <span data-ttu-id="b11bd-106">Pozměnění generovaného JavaScript pro zmenšení velikosti souborů&mdash;tento proces se označuje jako *minimalizace*.</span><span class="sxs-lookup"><span data-stu-id="b11bd-106">Mangled the generated JavaScript to reduce file size&mdash;a process known as *minification*.</span></span>
* <span data-ttu-id="b11bd-107">Zkopírovat zpracované soubory HTML, JavaScript a CSS z *src* k *wwwroot* adresáře.</span><span class="sxs-lookup"><span data-stu-id="b11bd-107">Copied the processed JavaScript, CSS, and HTML files from *src* to the *wwwroot* directory.</span></span>
* <span data-ttu-id="b11bd-108">Vložit následující prvky do *wwwroot/index.html* souboru:</span><span class="sxs-lookup"><span data-stu-id="b11bd-108">Injected the following elements into the *wwwroot/index.html* file:</span></span>
    * <span data-ttu-id="b11bd-109">A `<link>` značka, odkazující *wwwroot/main.\< Hodnota hash\>.css* souboru.</span><span class="sxs-lookup"><span data-stu-id="b11bd-109">A `<link>` tag, referencing the *wwwroot/main.\<hash\>.css* file.</span></span> <span data-ttu-id="b11bd-110">Tato značka je umístěna bezprostředně před uzavírací `</head>` značky.</span><span class="sxs-lookup"><span data-stu-id="b11bd-110">This tag is placed immediately before the closing `</head>` tag.</span></span>
    * <span data-ttu-id="b11bd-111">A `<script>` značky, odkazující minifikovaný *wwwroot/main.\< Hodnota hash\>.js* souboru.</span><span class="sxs-lookup"><span data-stu-id="b11bd-111">A `<script>` tag, referencing the minified *wwwroot/main.\<hash\>.js* file.</span></span> <span data-ttu-id="b11bd-112">Tato značka je umístěna bezprostředně před uzavírací `</body>` značky.</span><span class="sxs-lookup"><span data-stu-id="b11bd-112">This tag is placed immediately before the closing `</body>` tag.</span></span>