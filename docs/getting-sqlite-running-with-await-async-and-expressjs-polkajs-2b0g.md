# 💾使用 await/async 和 expressjs/polkajs 运行 Sqlite

> 原文：<https://dev.to/lampewebdev/getting-sqlite-running-with-await-async-and-expressjs-polkajs-2b0g>

❗❗❗❗ **这篇博文的灵感来自于我们在[实时视频流](https://twitch.tv/lampewebdev)的工作。如果你想了解我们在那边如何制作一个苗条的博客，或者你对网络开发有什么问题？欢迎每个问题，没有愚蠢的问题！我会尽我所能回答他们！要访问 twitch 页面，请单击👉[这里](https://twitch.tv/lampewebdev)👈。**

### 简介

SQLite 由许多大公司使用，如谷歌、Mozilla 或脸书。Android 和 iOS 应用程序将它用作本地数据库。它用于嵌入式设备，如宝马的 iDrive。甚至在 Flame 这样的恶意软件中也发现了它。如你所见，SQLite 可以用在很多场合和产品中！

### 要求

*   安装了最新版本的 NodeJS 和 NPM，或者至少支持异步/等待。
*   基本的 SQL 知识。基本！我们在这里不做任何花哨的事情
*   对异步/等待的基本理解。我推荐阅读我关于[异步/等待](https://dev.to/lampewebdev/i-promise-this-is-a-practical-guide-to-async--await-39ek)的文章
*   所有的命令都是 Unix 命令，如果你想在 windows 上运行它们，你需要把它们改成 Windows 版本。我强烈推荐 [WSL](https://en.wikipedia.org/wiki/Windows_Subsystem_for_Linux) 。

### 设置

我们需要做的第一件事是初始化一个新的 NPM 项目。让我们创建一个新的 git repo 并初始化 NPM。

```
mkdir sqlite-expressjs-async-await-blogpost
cd sqlite-expressjs-async-await-blogpost
git init
npm init -y 
```

这将创建一个名为`SQLite-expressjs-async-await-blogpost`的文件夹。然后我们将目录更改为刚刚创建的目录，初始化 git 并用`npm init`命令创建一个新的`package.json`。`-y`代表接受一切。

现在我们已经初始化了我们的项目，让我们安装需要的 NPM 包。

```
npm i --save polka sqlite-async 
```

是一个极小的、高性能的 Express.js 替代品。如果你知道 expressjs，那你就像在家里一样。我们在这里使用的是`sqlite-async` NPM 包，而不是`sqlite`或`sqlite3`，因为你可以从名字中看到这个包支持异步/等待开箱即用，我们不需要做一些巫术来让它运行。

### 跑步波尔卡

我们需要为我们的应用创建一个入口点。

```
touch server.js 
```

这将在文件夹中创建一个 server.js 文件，现在我们终于可以使用代码编辑器了。
先试着开始波尔卡吧！

```
// server.js
const polka = require('polka');

polka()
    .get('/', (req, res) => {
        res.end('<h1>hello world</h1>');
    })
    .listen(3000, err => {
        if (err) throw err;
        console.log(`> Running on localhost:3000`);
    }); 
```

我们首先需要要求`polka`，然后我们正在创建一个名为`/`的路由。这是根路由，浏览器通常不会在地址栏显示出来，例如当你在`localhost:3000`时，正确的地址是:`http://localhost:3000/`，但是谁想输入正确的地址呢？我们所描述的都是由`get()`函数完成的。`listen()`函数告诉 polka 监听端口`3000`，第二个参数是服务器启动时运行的回调。3000 随便你怎么换！你甚至可以做一个黑客，在`1337`上运行它。现在可以叫自己`Elite`；)

但是如何启动那个服务器呢？这很简单！
在项目文件夹的终端中，您可以键入:

```
node server.js 
```

这个命令会启动波尔卡，你可以去`localhost:3000`你应该会看到一个大胖子 **`hello world`** ！

### 创建一个空数据库

既然我们知道我们可以运行我们的服务器，我们可以设置 SQLite。

请记住，我们没有使用任何花哨的服务器自动重装。您需要在每次保存后将其关闭并重新运行。你可以在运行服务器的终端上按下`CTRL+C`，然后用`node server.js`重新运行服务器。

```
// server.js
const Database = require('sqlite-async') 
```

我们首先需要导入 sqlite-async，现在我们需要稍微重写一下我们的`server.js`,使它能够与 async/await
一起工作

```
// server.js
const main = async () => {
    try {
        db = await Database.open(":memory:");
    } catch (error) {
        throw Error('can not access sqlite database');
    }
    polka()
        .get('/', (req, res) => {
            res.end('<h1>hello world</h1>');
        })
        .listen(3000, err => {
            if (err) throw err;
            console.log(`> Running on localhost:3000`);
        });
}

main(); 
```

我们一步一步来。

我们根本没有改变`polka()`代码。
我们用一个异步语句将所有东西包装在一个胖箭头函数中，在文件的末尾，我们调用这个函数。我们需要这样做来使等待工作。

我们来说说这一行:

```
db = await Database.open(":memory:"); 
```

这条线是最大的新事物！我们正在“开放”一个新的数据库。该函数实际上检查是否已经有一个数据库，如果有，它只是连接到该数据库，如果现在有数据库，它创建一个新的，然后连接到它。`:memory:`意味着我们在计算机的 RAM 中创建数据库，而不是在文件系统中的某个地方，如果您希望您的数据在服务器崩溃或重新加载后仍然存在，您应该这样做！我们在这里使用`:memory:`是因为它更容易清理，因为你根本不用清理；).
因此，当这是成功的，我们有一个连接到我们的数据库！
`try/catch`的存在是因为如果出现未处理的错误，nodejs 将会崩溃！在履行承诺时，请始终使用`try/catch`！

### 创建一个空表！

现在我们有了一个数据库，我们还需要一个表。我们将创建包含以下列的名为 user 的表:

*   名字类型文本
*   姓氏类型文本

```
// server.js
// insert that code after the `database.open` try/catch
    try {
        await db.run(`
        CREATE TABLE user (
                    firstName TEXT,
                    lastName TEXT
        )
        `);    
    } catch (error) {
        throw Error('Could not create table')
    } 
```

代码将创建一个名为`user`的表，这个表将有两列文本类型的`firstName`和`lastName`。

### 插入一些数据

现在让我们向表中插入一些数据！

```
// server.js
// Insert this after the Create table try/catch
    try {
        const insertString = `
            INSERT INTO blogPosts 
            (firstName, lastName)
            VALUES (?,?)
        `;
        await db.run(insertString,
            "Michael",
            "Lazarski"
        );
    } catch (error) {
        throw Error('Could not insert new user');
    } 
```

好的，这个查询有两个部分。`const insertString`和实际运行命令以及我们想要插入的数据。

```
INSERT INTO users(firstName, lastName) 
```

这告诉 SQLite 我们想要插入数据库，第一个字段是 firstName，第二个字段是 lastName。

```
 VALUES (?, ?) 
```

这一行令人兴奋。`VALUES`这里意味着我们需要指定我们想要插入到表格中的值。把它想象成一个参数列表，你可以把它传递给一个函数。这也与`users(firtName, lastName)`线有联系。这里的秩序很重要！在这种情况下，第一个问号是名，第二个问号是姓。但是为什么是`?`。再看一下`db.run()`功能。第一个参数是我们的查询。第二个和第三个的顺序与问号的顺序相同。这有两个跳跃。在插入行中，我们告诉我们想要在`VALUES`行中插入什么，我们告诉 SQLite 我们想要插入`db.run()`函数的第二个和第三个参数。这样做是很好的实践，因为`sqlite-async` npm 包还会为您准备字符串和转义字符，您不能像`'`或其他特殊字符那样简单地插入它们。

### 获取数据结束显示在页面上

我们现在需要查询我们的数据，然后将其发送回客户端。
下面的代码可以做到这一点:

```
// server.js
// change the .get('/') function
polka()
    .get('/', async (req, res) => {
        const {firstName, lastName} = 
            await db.get("SELECT firstName, lastName FROM user");
        res.end(`<h1>hello ${firstName}  ${lastName} </h1>`);
    }) 
```

我们做的第一件事是将第二个参数(一个粗箭头函数)设为异步，这样我们就可以使用 await。我们可以在这里使用一个简单的 Select，因为我们的表中只有一行。我们再次从用户表中选择 firstName 和 lastName，因为我们只是用`db.get()`函数返回一个对象，所以我们可以析构它。最后一步是使用模板文本来创建我们的小 HTML 示例。

### 可选:搜索特定用户

假设您现在有很多用户，您想在数据库中找到第一个`Michael`。为此，您需要稍微改变一下`SELECT`。

```
await db.get(`SELECT firstName, lastName 
                FROM user 
                WHERE firstName LIKE ?`,
                "%Michael%"); 
```

这里唯一的新东西是`WHERE`和`LIKE`。我们在这里所做的是搜索我们的名字与`Michael`匹配的第一个条目。前面和后面的%表示什么`Michael`可以是该名称中的任何位置。例如，`MMichael`或`Michaels`也会匹配。

### 压轴代码

如果你想查看大结局代码，你可以在下面的 [github repo](https://github.com/lampewebdev/sqlite-expressjs-async-await-blogpost) 中找到它

如果你能为我做以下事情，那将对我有所帮助！
去 [Twitch](https://dev.to/twitch_live_streams/lampewebdev) 给我留个关注！如果只有少数人会这样做，那么这对我意味着整个世界！❤❤❤😊

**👋说你好！**[insta gram](https://www.instagram.com/lampewebdev/)|[Twitter](https://twitter.com/lampewebdev)|[LinkedIn](https://www.linkedin.com/in/michael-lazarski-25725a87)|[Medium](https://medium.com/@lampewebdevelopment)|[Twitch](https://dev.to/twitch_live_streams/lampewebdev)|[YouTube](https://www.youtube.com/channel/UCYCe4Cnracnq91J0CgoyKAQ)