# 我们的第一个应用 {#firstapp}

![Memos，一个极简记事本应用](images/originals/memos-app.png)

在这一章节，我们将建立一个简单的 **Memos** 应用，这是一个做笔记的应用。在编码前先让我们来看看这个应用的功能。

该应用有三个界面。第一个是主界面，有一个被存储的你的笔记的标题的列表。当你点击一个笔记（或添加一个新笔记）你将转到详情界面，可以让你编辑这个笔记的内容和标题。如下图所示。

![Memos，编辑界面](images/originals/memos-editing-screen.png)

在上面显示的界面中，用户点击垃圾桶图标就可以选择是否删除被选中的笔记。这将引发一个确认对话框被显示出来。

![Memos，笔记删除确认界面](images/originals/memos-delete-screen.png)

可用的Memos源代码在[Memos的Github仓库](https://github.com/soapdog/memos-for-firefoxos)上（也可用[.zip](https://github.com/soapdog/memos-for-firefoxos/archive/master.zip)文件）。我建议你下载这个文件，这样更易于接下去的学习。可用源代码的另一份副本在[本书Github仓库](https://github.com/soapdog/firefoxos-quick-guide)里的 **code** 文件夹中。

Memos使用[IndexedDB](https://developer.mozilla.org/en-US/docs/IndexedDB/Using_IndexedDB)存储笔记并使用[盖亚构建模块](http://buildingfirefoxos.com/building-blocks)构建接口。以后更新本书时，我将谈到更多的盖亚构建模块，但在第一本中我只会使用它们。关于它们，你可以点开上面的链接学习更多的内容，以及它们提供的用户界面工具。

第一步是创建应用程序的文件夹，我们来给这个文件夹命名为**memos**。

## 创建应用清单（Manifest）

Memos的清单是优雅而简介的。在**memos**文件夹中创建名为**manifest.webapp**的文件。清单是描述应用的[JSON](http://json.org)文件。在该文件中我们写一些东西，比如应用的名称、开发者是谁、使用的图标是什么、运行应用的文件是什么、想要使用什么特权API，还有其他的。

在下面我们能看到Memos应用清单的内容。复制这些数据时要注意，因为非常容易在错误的地方写逗号导致JSON格式错误。这里有许多你能用来验证JSON文件的工具，不过这还有一个特别的可以构建特殊的应用清单。你可以从[http://appmanifest.org/](http://appmanifest.org/)找到这个在线工具。想学习更多关于应用清单的信息，阅读[MDN相关信息的页面](https://developer.mozilla.org/docs/Apps/Manifest)。

<<[Memos清单文件 (*manifest.webapp*)](code/memos/manifest.webapp)

我们来从上面的清单中看看字段。

|字段		|描述                                                                        |
|-----------|-----------------------------------------------------------------------------------|
|name		|这是应用的名称。		                                                |
|version	|这是应用的当前版本。				    |
|launch_path|被用于启动你的应用的文件是什么。					                    |
|permissions|你的应用请求什么API许可。更多相关信息在下面。				|
|developer  |谁开发了这个应用。 													|
|icons		|应用使用的许多不同尺寸的图标。									|

该清单最有趣的部分是permissions字段，我们请求*storage*许可来允许我们无存储限制的使用IndexedDB[^storage-permission]（多亏那个许可，我们才能尽可能多的存储我们想要的笔记，尽管我们应该注意不要过多使用用户的磁盘空间！）。

[^storage-permission]: 学习更多关于permissions许可的信息，阅读[MDN关于许可的页面](https://developer.mozilla.org/en-US/docs/Web/Apps/App_permissions)。

现在清单已经准备好了，我们该看看HTML了。

## 构建HTML

在我们在HTML上开始工作前，我们先来简短的谈谈[盖亚构建模块](http://buildingfirefoxos.com/building-blocks)，这是一个有着Firefox OS*感观*的可重用的CSS和JS集合，我们可以在我们自己的应用中使用。

正如网页中，你不需要在你自己的应用中使用Firefox OS的*感观*。使用或不使用盖亚构建模块是你个人的决定，而且一个良好的应用应该具有它自己的独特风格和用户体验。重要的是应该了解到，即使不使用盖亚感观你的应用也不会在Firefox Marketplace应用市场中遭受任何类型的偏见和处罚。我使用它，因为我不是个良好的设计师，所以现成的设计用户界面的工具很吸引我（要么雇佣一个设计师）。

我们在应用中使用的HTML的结构是被盖亚构建模块所采用的模式，每个界面是一个`<section>`，该元素使用某个预处理格式。如果你没有准备好，可以从[memos仓库](https://github.com/soapdog/memos-for-firefoxos)下载源代码，使用这些文件（包括构建模块）。对于那些不信任git和GitHub的，这个[.zip file](https://github.com/soapdog/memos-for-firefoxos/archive/master.zip)文件也是可用的。

W> 注意：我在这个应用中使用的盖亚构建模块的版本不是从Mozilla获取的最新的。不幸地是，若尝试更新到当前的版本将破坏该Memos应用。在你自己的项目中，无论如何一定要使用最新版本的盖亚构建模块。

### 包含的构建模块

在做任何事之前，在你创建的**memos**文件夹中复制从Memos仓库下载获取到的**shared**和**styles**文件。这将允许在我们的应用中使用盖亚构建模块。

让我们从我们的**index.html**文件开始，包括所需的一些东西。

~~~~~~~~
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <link rel="stylesheet" type="text/css" href="/style/base.css" />
    <link rel="stylesheet" type="text/css" href="/style/ui.css" />
    <link rel="stylesheet" type="text/css" href="/style/building_blocks.css" />
    <link rel="stylesheet" type="text/css" href="shared/style/headers.css" />
    <link rel="stylesheet" type="text/css" href="shared/style_unstable/lists.css" />
    <link rel="stylesheet" type="text/css" href="shared/style_unstable/toolbars.css" />
    <link rel="stylesheet" type="text/css" href="shared/style/input_areas.css" />
    <link rel="stylesheet" type="text/css" href="shared/style/confirm.css" />
    <title>Memos</title>
</head>
~~~~~~~~

在*行01*中我们声明DOCTYPE为HTML5。从*行05*到*行15*，我们引入各种在我们的应用中使用的组件的CSS，比如头部、列表、文本输入字段和其他。

### 建立主界面

现在我们可以开始建立各种界面。像之前提到的一样，每个界面在我们的应用中是一个在`<body>`中的`<section>`。body标签必须有一个*role*属性，它的值为*application*，因为那是被CSS选择器用于构建接口的，所以我们的body标签是`<body role="application">`。让我们建立第一个界面并声明我们的body标签。

~~~~~~~~
<body role="application">

<section role="region" id="memo-list">
    <header>
        <menu type="toolbar">
            <a id="new-memo" href="#"><span class="icon icon-add">add</span></a>
        </menu>
        <h1>Memos</h1>
    </header>
    <article id="memoList" data-type="list"></article>
</section>
~~~~~~~~

我们的界面中有一个`<header>`包括添加新笔记按钮和应用名称。界面中还有`<article>`用来放置被存储笔记的列表。当我们延伸到JavaScript实现部分时，我们将使用按钮和文章ID捕获事件。

注意，每个界面就相当于是一个HTML块。构建使用不同语言的相同界面通常需要做许多其他工作。我们正在做的事就是声明我们的容器并在我们需要在之后引用它们时，获取它们的ID。

现在主界面做好了，我们来构建编辑界面。

### 建立编辑界面

编辑界面会更复杂一些，因为还放置了一个对话框，用于当用户尝试删除一个笔记的时候。

~~~~~~~~
<section role="region" id="memo-detail" class="skin-dark hidden">
    <header>
        <button id="back-to-list"><span class="icon icon-back">back</span>
        </button>
        <menu type="toolbar">
            <a id="share-memo" href="#"><span class="icon icon-share">share</span>
            </a>
        </menu>
        <form action="#">
            <input id="memo-title" placeholder="Memo Title" required="required" type="text">
            <button type="reset">Remove text</button>
        </form>
    </header>
    <p id="memo-area">
        <textarea placeholder="Memo content" id="memo-content"></textarea>
    </p>
    <div role="toolbar">
        <ul>
            <li>
                <button id="delete-memo" class="icon-delete">Delete</button>
            </li>
        </ul>
    </div>
    <form id="delete-memo-dialog" role="dialog" data-type="confirm" class="hidden">
        <section>
            <h1>Confirmation</h1>
            <p>Are you sure you want to delete this memo?</p>
        </section>
        <menu>
            <button id="cancel-delete-action">Cancel</button>
            <button id="confirm-delete-action" class="danger">Delete</button>
        </menu>
    </form>
</section>
~~~~~~~~

在界面的顶部，仍然出现了`<header>`元素，里面有：

 * 返回按钮，回到主界面。
 * 文本字段用来放得到的笔记的标题，
 * 并且按钮被用于通过电子邮件分享笔记。

在下面的顶部工具栏中，我们有一段放着一个 `<textarea>` ，那写着笔记的内容和另一个工具栏，包括用于删除当前浏览的笔记的垃圾箱按钮。

这三个元素和它们的子节点都是编辑界面。在它们后面我们有 `<form>` ，用来作为包含确认界面的对话框，当他或她试图删除一个笔记时就为用户呈现它。该对话框非常简单，它仅包含确认提示的文本和两个按钮；一个删除笔记而另一个取消这个动作。

现在我们闭合了这个 `<section>` 我们实现了所有的界面并且剩下的HTML代码只有引入JavaScript文件和闭合html文件了。

~~~~~~~~
<script src="/js/model.js"></script>
<script src="/js/app.js"></script>
</body>
</html>
~~~~~~~~

## 加工JavaScript代码

现在我们通过添加JavaScript为我们的应用注入活力。为了更好的组织代码，我将JavaScript代码划分到了两个文件里：

* **model.js:** 包括常规的处理存储和笔记检索，但不包括任何应用程序逻辑或任何有关接口或数据字段的东西。理论上我们能在其他需要文本笔记的应用程序重用相同文件。
* **app.js:** 为HTML元素绑定它们的事件处理器以及包括应用程序逻辑。

两个文件都应被放在一个**js**文件夹，之后还有**style** 和 **shared** 文件夹。

### model.js

我们将使用[IndexedDB](https://developer.mozilla.org/en-US/docs/IndexedDB/Using_IndexedDB)来存储我们的笔记。从我们在应用程序的清单中要求了*存储*许可，我们想存多少就存储多少。无论如何，我们不该滥用它！Firefox OS设备通常存储空间非常有限，所以你总需要考虑周全你要存储的数据是什么（如果你的应用使用太多的存储空间，用户会删除它并且它的下载率也将减少）。存储过多的数据有点性能损失，使你的应用感觉很慢。当你提交一个应用到Firefox OS marketplace应用市场时也请注意，审查员们会询问你为什么需要无限存储空间，如果你没有正当的原因，你的应用将被拒。

下面显示的*model.js*的这部分代码是可响应打开链接和创建存储的。

A> 重要提示：该代码被写的浅显易懂并且不能表现最佳JS编程实践。使用了一些全局变量（我真该下地狱）等花絮。错误处理代码基本上是不存在的。本书的主要目的是教授为Firefox OS开发应用的*工作流*，而不是教最好的JS模式。话虽然这么说，根据反馈我会在本书中更新这部分代码，以更好的反映最佳实践，只要有够多的人认为这不会影响初学者。

~~~~~~~
var dbName = "memos";
var dbVersion = 1;

var db;
var request = indexedDB.open(dbName, dbVersion);

request.onerror = function (event) {
    console.error("Can't open indexedDB!!!", event);
};
request.onsuccess = function (event) {
    console.log("Database opened ok");
    db = event.target.result;
};

request.onupgradeneeded = function (event) {

    console.log("Running onUpgradeNeeded");

    db = event.target.result;

    if (!db.objectStoreNames.contains("memos")) {

        console.log("Creating objectStore for memos");

        var objectStore = db.createObjectStore("memos", {
            keyPath: "id",
            autoIncrement: true
        });
        objectStore.createIndex("title", "title", {
            unique: false
        });

        console.log("Adding sample memo");
        var sampleMemo1 = new Memo();
        sampleMemo1.title = "Welcome Memo";
        sampleMemo1.content = "This is a note taking app. Use the plus sign in the topleft corner of the main screen to add a new memo. Click a memo to edit it. All your changes are automatically saved.";

        objectStore.add(sampleMemo1);
    }
}
~~~~~~~

A> 重要提示：为了大局，再次原谅我，这仅是教学资源。另一个细节是我删除了本书中源代码的注释来节省空间。如果你从GitHub找出源代码你就会获得全部的注释。

上面的代码创建了一个*db*对象和一个*request*对象。*db*对象在源代码中被其他函数用于笔记存储操作。

在`request.onupgradeneeded`方法的实现中我们还创建了欢迎笔记。当应用程序第一次运行时该方法就会被执行（或当数据库版本改变时）。该方法就是一旦应用程序第一次启动，数据库就会被初始化并生成一个欢迎笔记。


与我们的连接打开以及初始化存储的同时来实现笔记操作的基础功能。

~~~~~~~~
function Memo() {
    this.title = "Untitled Memo";
    this.content = "";
    this.created = Date.now();
    this.modified = Date.now();
}

function listAllMemoTitles(inCallback) {
    var objectStore = db.transaction("memos").objectStore("memos");
    console.log("Listing memos...");

    objectStore.openCursor().onsuccess = function (event) {
        var cursor = event.target.result;
        if (cursor) {
            console.log("Found memo #" + cursor.value.id + " - " + cursor.value.title);
            inCallback(null, cursor.value);
            cursor.continue();
        }
    };
}

function saveMemo(inMemo, inCallback) {
    var transaction = db.transaction(["memos"], "readwrite");
    console.log("Saving memo");

    transaction.oncomplete = function (event) {
        console.log("All done");
    };

    transaction.onerror = function (event) {
        console.error("Error saving memo:", event);
        inCallback({
            error: event
        }, null);

    };

    var objectStore = transaction.objectStore("memos");

    inMemo.modified = Date.now();

    var request = objectStore.put(inMemo);
    request.onsuccess = function (event) {
        console.log("Memo saved with id: " + request.result);
        inCallback(null, request.result);

    };
}

function deleteMemo(inId, inCallback) {
    console.log("Deleting memo...");
    var request = db.transaction(["memos"], "readwrite").objectStore("memos").delete(inId);

    request.onsuccess = function (event) {
        console.log("Memo deleted!");
        inCallback();
    };
}
~~~~~~~~

On the piece of code above we create a constructor function that creates new Memos with some fields already initialized. After that we implement functions for listing, saving and removing notes. Many of these functions receive a callback parameter called `inCallback` which is a function to be called after the function does its thing. This is needed due to the asynchronous nature of IndexedDB. All callbacks have the same signature which is `callback(error, value)` where one of the values is null depending on the outcome of the previous function.

A> Since this is a beginner book I've opted not to use [*Promises*](https://developer.mozilla.org/en-US/docs/Mozilla/JavaScript_code_modules/Promise.jsm/Promise) since many beginners are not familiar with the concept. I recommend using such concepts to create easier to maintain code that is more pleasant to read.

Now that our note storage and manipulation functions are ready, let's implement our app logic in a file called **app.js**.

### app.js

This file will contain our app logic. Since the source code is too large for me to place it all at once on the book, I will break it in parts and explain each part piece by piece.

~~~~~~~~
var listView, detailView, currentMemo, deleteMemoDialog;

function showMemoDetail(inMemo) {
    currentMemo = inMemo;
    displayMemo();
    listView.classList.add("hidden");
    detailView.classList.remove("hidden");
}


function displayMemo() {
    document.getElementById("memo-title").value = currentMemo.title;
    document.getElementById("memo-content").value = currentMemo.content;
}

function shareMemo() {
    var shareActivity = new MozActivity({
        name: "new",
        data: {
            type: "mail",
            body: currentMemo.content,
            url: "mailto:?body=" + encodeURIComponent(currentMemo.content) + "&subject=" + encodeURIComponent(currentMemo.title)

        }
    });
    shareActivity.onerror = function (e) {
        console.log("can't share memo", e);
    };
}

function textChanged(e) {
    currentMemo.title = document.getElementById("memo-title").value;
    currentMemo.content = document.getElementById("memo-content").value;
    saveMemo(currentMemo, function (err, succ) {
        console.log("save memo callback ", err, succ);
        if (!err) {
            currentMemo.id = succ;
        }
    });
}

function newMemo() {
    var theMemo = new Memo();
    showMemoDetail(theMemo);
}
~~~~~~~~

At the beginning we declare some global variables (yuck!!!) to hold references to some DOM Elements that we want to use later inside some functions. The most interesting global is `currentMemo` which is an object that holds the current note that the user is reading.

The `showMemoDetail()` and `displayMemo()` functions work together. The first one loads the selected note into the `currentMemo` and manipulates the CSS of the elements so that the editing screen is shown. The second one picks the content from the `currentMemo` variable and places it on the screen. We could do both things on the same function but having them separate makes it easier to experiment with new implementations.

The `shareMemo()` function uses a [WebActivity](https://hacks.mozilla.org/2013/01/introducing-web-activities/) to open the email application with a new message pre-filled with the selected notes content. 

The `textChanged()` function picks the data from the entry fields and place them into the `currentMemo` object and then saves the note. This is done because the application is an `auto-save` app where your content is always saved. All alterations on the content or title of the note will trigger this function and the note will always be saved on the IndexedDB storage.

The `newMemo()` function creates a new note and opens the editing screen with it.

~~~~~~~~
function requestDeleteConfirmation() {
    deleteMemoDialog.classList.remove("hidden");
}

function closeDeleteMemoDialog() {
    deleteMemoDialog.classList.add("hidden");
}

function deleteCurrentMemo() {
    closeDeleteMemoDialog();
    deleteMemo(currentMemo.id, function (err, succ) {
        console.log("callback from delete", err, succ);
        if (!err) {
            showMemoList();
        }
    });
}

function showMemoList() {
    currentMemo = null;
    refreshMemoList();
    listView.classList.remove("hidden");
    detailView.classList.add("hidden");
}
~~~~~~~~

The `requestDeleteConfirmation()` function is responsible for showing the note removal confirmation dialog.

The `closeDeleteMemoDialog()` and `deleteCurrentMemo()` are triggered by the buttons on the removal confirmation dialog.

The `showMemoList()` function does some clean up before showing the list of stored notes. For example, it cleans the content of `currentMemo` since we're not reading any memo yet.

~~~~~~~~
function refreshMemoList() {
    if (!db) {
        // HACK:
        // this condition may happen upon first time use when the
        // indexDB storage is under creation and refreshMemoList()
        // is called. Simply waiting for a bit longer before trying again
        // will make it work.
        console.warn("Database is not ready yet");
        setTimeout(refreshMemoList, 1000);
        return;
    }
    console.log("Refreshing memo list");

    var memoListContainer = document.getElementById("memoList");


    while (memoListContainer.hasChildNodes()) {
        memoListContainer.removeChild(memoListContainer.lastChild);
    }

    var memoList = document.createElement("ul");
    memoListContainer.appendChild(memoList);

    listAllMemoTitles(function (err, value) {
        var memoItem = document.createElement("li");
        var memoP = document.createElement("p");
        var memoTitle = document.createTextNode(value.title);

        memoItem.addEventListener("click", function (e) {
            console.log("clicked memo #" + value.id);
            showMemoDetail(value);

        });

        memoP.appendChild(memoTitle);
        memoItem.appendChild(memoP);
        memoList.appendChild(memoItem);


    });
}
~~~~~~~~

The `refreshMemoList()` function modifies the DOM by building element by element the list of notes that is displayed on the screen. It would be a lot easier to use some templating aid such as [handlebars](http://handlebarsjs.com/) or [underscore](http://underscorejs.org/) but since this app is built using nothing but *vanilla javascript* we're doing everything by hand. This function is called by `showMemoList()` that was shown above.

These are all the functions used by our app. The only part of the code that is missing is the initialization of the event handlers and the initial call of `refreshMemoList()`.

~~~~~~~
window.onload = function () {
    // elements that we're going to reuse in the code
    listView = document.getElementById("memo-list");
    detailView = document.getElementById("memo-detail");
    deleteMemoDialog = document.getElementById("delete-memo-dialog");

    // All the listeners for the interface buttons and for the input changes
    document.getElementById("back-to-list").addEventListener("click", showMemoList);
    document.getElementById("new-memo").addEventListener("click", newMemo);
    document.getElementById("share-memo").addEventListener("click", shareMemo);
    document.getElementById("delete-memo").addEventListener("click", requestDeleteConfirmation);
    document.getElementById("confirm-delete-action").addEventListener("click", deleteCurrentMemo);
    document.getElementById("cancel-delete-action").addEventListener("click", closeDeleteMemoDialog);
    document.getElementById("memo-content").addEventListener("input", textChanged);
    document.getElementById("memo-title").addEventListener("input", textChanged);

    // the entry point for the app is the following command
    refreshMemoList();

};
~~~~~~~

Now all files are ready and we can begin trying our application on the simulator.

## Testing the app on the simulator

Before we try our application on the simulator we'd better check out if the files are in the correct place. Your memos folder should look like this one:

![List of files used by Memos](images/originals/memos-file-list.png)

If you have a hunch that you wrote something wrong, just compare your version with the one on [the memos github repository](https://github.com/soapdog/memos-for-firefoxos) (There is also a copy of the source code in a folder called **code** on the [book repository](https://github.com/soapdog/guia-rapido-firefox-os) ).

To open the *Simulator Dashboard* go to the menu for **Tools -> Web Developer -> Firefox OS Simulator**.

![How to open simulator dashboard](images/originals/tools-web-developer-simulator.png)

With the dashboard open, click the **Add Directory** button and browse to where you placed the memos files and select the app manifest.

![Adding a new app](images/originals/simulator-add-directory.png)

If everything works as expected you will see the Memos app on the list of apps.

![Memos showing on the dashboard](images/originals/memos-on-dashboard-display.png)

When you add a new application, the simulator will launch with your new app running - allowing you to test it. Now you can test all the features for Memos. 

Congratulations! you created and tested your first app. Its not a complex or revolutionary app - but I hope it helped you understand the development workflow of FireFox OS. As you can see, it's not very different from standard Web development.  

Remember that whenever you alter some of the source files you need to press the **Refresh** button to update the copy of the app that is stored on the simulator.

## Summary

In this chapter we built our first application for Firefox OS and saw it running on the simulator. In the next chapter we're going to check out the developer tools that comes bundled with Firefox, they will make your life a lot easier when developing applications.

