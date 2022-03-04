---
title: Retrieve a window handle (HWND)
description: This topic shows you how, in a desktop app, to retrieve the window handle for a window.
ms.topic: article
ms.date: 03/02/2022
keywords: Windows, App, SDK, desktop, C#, C++, cpp, window, handle, HWND, Windows UI Library, WinUI
ms.author: stwhi
author: stevewhims
ms.localizationpriority: medium
---

# Retrieve a window handle (HWND)

This topic shows you how, in a desktop app, to retrieve the window handle for a window. The scope covers [Windows UI Library (WinUI) 3](/windows/apps/winui/winui3/), [Windows Presentation Foundation (WPF)](/dotnet/desktop/wpf/), and [Windows Forms (WinForms)](/dotnet/desktop/winforms/) apps; code examples are presented in C# and [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/).

The development and UI frameworks listed above are (behind the scenes) built on the [Win32 API](/windows/win32/). In Win32, a [window](/windows/win32/winmsg/about-windows) object is identified by a value known as a [window handle](/windows/win32/winmsg/about-windows#window-handle). And the type of a window handle is an **[HWND](/windows/win32/winprog/windows-data-types)** (although it surfaces in C# as an [**IntPtr**](/dotnet/api/system.intptr)). In any case, you'll hear the term **HWND** used as a shorthand for *window handle*.

There are several reasons to retrieve the **HWND** for a window in your WinUI 3, WPF, or WinForms desktop app. One example is to use the **HWND** to interoperate with certain system types in the **Windows.\*** namespaces. For more info, see [Display Windows.\*-namespace UI objects](/windows/apps/develop/ui-input/display-ui-objects).

## WinUI 3 (also WPF/WinForms with .NET 5 or later) with C#

> [!NOTE]
> The code example in this section uses the **WinRT.Interop.WindowNative** C# interop class. If you target .NET 5 or later, then you can use that class in a WPF or WinForms project. For info about setting up your project to do that, see [Call WinRT COM interop interfaces from a .NET 5+ app](/windows/apps/desktop/modernize/winrt-com-interop-csharp).

The C# code below shows how to retrieve the window handle (HWND) for a WinUI 3 [Window](/windows/winui/api/microsoft.ui.xaml.window) object. This example calls the **GetWindowHandle** method on the **WinRT.Interop.WindowNative** C# interop class. For more info about the C# interop classes, see [Call WinRT COM interop interfaces from a .NET 5+ app](/windows/apps/desktop/modernize/winrt-com-interop-csharp).

```csharp
// MainWindow.xaml.cs
private async void myButton_Click(object sender, RoutedEventArgs e)
{
    // Retrieve the window handle (HWND) of the current WinUI 3 window.
    var hWnd = WinRT.Interop.WindowNative.GetWindowHandle(this);
}
```

## WinUI 3 with C++

The C++/WinRT code below shows how to retrieve the window handle (HWND) for a WinUI 3 [Window](/windows/winui/api/microsoft.ui.xaml.window) object. This example calls the [**IWindowNative::get_WindowHandle**](/windows/windows-app-sdk/api/win32/microsoft.ui.xaml.window/nf-microsoft-ui-xaml-window-iwindownative-get_windowhandle) method.

```cppwinrt
// pch.h
...
#include <microsoft.ui.xaml.window.h>

// MainWindow.xaml.cpp
void MainWindow::myButton_Click(IInspectable const&, RoutedEventArgs const&)
{
    // Retrieve the window handle (HWND) of the current WinUI 3 window.
    auto windowNative{ this->try_as<::IWindowNative>() };
    winrt::check_bool(windowNative);
    HWND hWnd{ 0 };
    windowNative->get_WindowHandle(&hWnd);
}
```

## WPF (earlier than .NET 5) with C#

The C# code below shows how to retrieve the window handle (HWND) for a WPF window object. This example uses the [**WindowInteropHelper**](/dotnet/api/system.windows.interop.windowinterophelper) class.

```csharp
// MainWindow.xaml.cs
private void Button_Click(object sender, RoutedEventArgs e)
{
    var wih = new System.Windows.Interop.WindowInteropHelper(this);
    var hWnd = wih.Handle;
}
```

## WinForms (earlier than .NET 5) with C#

The C# code below shows how to retrieve the window handle (HWND) for a WinForms form object. This example uses the [**NativeWindow.Handle**](/dotnet/api/system.windows.forms.nativewindow.handle) property.

```csharp
// Form1.cs
private void button1_Click(object sender, EventArgs e)
{
    var hWnd = this.Handle;
}
```

## Related topics

* [Display Windows.\*-namespace UI objects](/windows/apps/develop/ui-input/display-ui-objects)
* [Windows UI Library (WinUI) 3](/windows/apps/winui/winui3/)
* [Windows Presentation Foundation (WPF)](/dotnet/desktop/wpf/)
* [Windows Forms (WinForms)](/dotnet/desktop/winforms/)
* [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/)
* [Win32 API](/windows/win32/)