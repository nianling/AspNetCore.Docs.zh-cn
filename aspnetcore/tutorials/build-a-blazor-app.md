---
title: 生成 Blazor 待办事项列表应用
author: guardrex
description: 逐步生成 Blazor 应用。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 07/02/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: tutorials/build-a-blazor-app
ms.openlocfilehash: 174a8e561701bb3ebd68ed05e42dfc3d70a9b450
ms.sourcegitcommit: 14c3d111f9d656c86af36ecb786037bf214f435c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/09/2020
ms.locfileid: "86176223"
---
# <a name="build-a-blazor-todo-list-app"></a>生成 Blazor 待办事项列表应用

作者：[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)

本教程演示如何生成和修改 Blazor 应用。 您将学习如何：

> [!div class="checklist"]
> * 创建待办事项列表 Blazor 应用项目
> * 修改 Razor 组件
> * 在组件中使用事件处理和数据绑定
> * 在 Blazor 应用中使用依赖关系注入 (DI) 和路由

在本教程结束时，你将拥有一个正常运行的待办事项列表应用。

1. 在命令行界面中创建名为 `TodoList` 的新 Blazor 应用：

   ```dotnetcli
   dotnet new blazorserver -o TodoList
   ```

   上述命令将创建一个名为 `TodoList` 的文件夹来保存应用。 使用以下命令将目录切换到 `TodoList` 文件夹：

   ```dotnetcli
   cd TodoList
   ```

1. 使用以下命令向 `Pages` 文件夹中的应用添加一个新的 `Todo` Razor 组件：

   ```dotnetcli
   dotnet new razorcomponent -n Todo -o Pages
   ```

   > [!IMPORTANT]
   > Razor 组件文件名要求首字母大写，因此请确认 `Todo` 组件的文件名以大写字母 `T` 开头。

1. 在 `Pages/Todo.razor` 中为组件提供初始标记：

   ```razor
   @page "/todo"

   <h3>Todo</h3>
   ```

1. 将 `Todo` 组件添加到导航栏。

   `NavMenu` 组件 (`Shared/NavMenu.razor`) 用于应用的布局。 布局是可以避免应用中出现重复内容的组件。

   通过在 `Shared/NavMenu.razor` 文件中的现有列表项下添加以下列表项标记，为 `Todo` 组件添加一个 `<NavLink>` 元素：

   ```razor
   <li class="nav-item px-3">
       <NavLink class="nav-link" href="todo">
           <span class="oi oi-list-rich" aria-hidden="true"></span> Todo
       </NavLink>
   </li>
   ```

1. 重新生成并运行应用。 访问新的“待办事项”页面，确认指向 `Todo` 组件的链接有效。

1. 向项目的根目录添加 `TodoItem.cs` 文件，以保存一个用于表示待办项的类。 为 `TodoItem` 类使用以下 C# 代码：

   [!code-csharp[](build-a-blazor-app/samples_snapshot/3.x/TodoItem.cs)]

1. 返回到 `Todo` 组件 (`Pages/Todo.razor`)：

   * 在 `@code` 块中为待办项添加一个字段。 `Todo` 组件使用此字段来维护待办项列表的状态。
   * 添加无序列表标记和 `foreach` 循环，以将每个待办项呈现为列表项 (`<li>`)。

   [!code-razor[](build-a-blazor-app/samples_snapshot/3.x/ToDo4.razor?highlight=5-10,12-14)]

1. 该应用需要 UI 元素来将待办项添加到列表。 在未排序列表 (`<ul>...</ul>`) 下方添加一个文本输入 (`<input>`) 和一个按钮 (`<button>`)：

   [!code-razor[](build-a-blazor-app/samples_snapshot/3.x/ToDo5.razor?highlight=12-13)]

1. 重新生成并运行应用。 选择“`Add todo`”按钮时没有任何反应，因为没有事件处理程序连接到该按钮。

1. 向 `Todo` 组件添加 `AddTodo` 方法，并使用 `@onclick` 属性注册该方法以选择按钮。 选择按钮时，会调用 `AddTodo` C# 方法：

   [!code-razor[](build-a-blazor-app/samples_snapshot/3.x/ToDo6.razor?highlight=2,7-10)]

1. 要获得新待办项标题，请在 `@code` 块顶部添加 `newTodo` 字符串字段，并使用 `<input>` 元素中的 `bind` 属性将其绑定到文本输入的值：

   [!code-razor[](build-a-blazor-app/samples_snapshot/3.x/ToDo7.razor?highlight=2)]

   ```razor
   <input placeholder="Something todo" @bind="newTodo" />
   ```

1. 更新 `AddTodo` 方法，将具有指定标题的 `TodoItem` 添加到列表。 通过将 `newTodo` 设置为空字符串来清除文本输入的值：

   [!code-razor[](build-a-blazor-app/samples_snapshot/3.x/ToDo8.razor?highlight=19-26)]

1. 重新生成并运行应用。 在待办项列表中添加一些待办项以测试新代码。

1. 每个待办项的标题文本都可以编辑，复选框可以帮助用户跟踪已完成的项。 为每个待办项添加一个复选框输入，并将它的值绑定到 `IsDone` 属性。 将 `@todo.Title` 更改为绑定到 `@todo.Title` 的 `<input>` 元素：

   [!code-razor[](build-a-blazor-app/samples_snapshot/3.x/ToDo9.razor?highlight=5-6)]

1. 若要验证这些值是否已绑定，请更新 `<h3>` 标头以显示尚未完成的待办项计数（`IsDone` 是 `false`）。

   ```razor
   <h3>Todo (@todos.Count(todo => !todo.IsDone))</h3>
   ```

1. 已完成的 `Todo` 组件 (`Pages/Todo.razor`)：

   [!code-razor[](build-a-blazor-app/samples_snapshot/3.x/Todo.razor)]

1. 重新生成并运行应用。 添加待办项以测试新代码。

## <a name="next-steps"></a>后续步骤

在本教程中，你将了解：

> [!div class="checklist"]
> * 创建待办事项列表 Blazor 应用项目
> * 修改 Razor 组件
> * 在组件中使用事件处理和数据绑定
> * 在 Blazor 应用中使用依赖关系注入 (DI) 和路由

了解用于 ASP.NET Core Blazor 的工具：

> [!div class="nextstepaction"]
> <xref:blazor/tooling>
