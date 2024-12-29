---
title: MFC VC++ 实现消息转发接收
date: 2024-12-29 17:29:04
description: 不同对话框实现消息转发接收机制
tags: 
  - VC++
  - MFC
categories:
password:
message:
cover: https://visualstudio.microsoft.com/wp-content/uploads/2019/02/BrandVisualStudioWin2019-2.svg
top_img: https://visualstudio.microsoft.com/wp-content/uploads/2019/02/BrandVisualStudioWin2019-2.svg
---

### 使用 `AfxGetMainWnd()` 和消息映射

这种方法适用于 `CResumeManagementSystemView` 是主框架窗口的一部分的情况。你可以使用 `AfxGetMainWnd()` 获取主窗口指针，然后发送消息给它。如果你的视图是从 `CView` 或 `CFormView` 派生的，可以通过主窗口找到视图并发送消息。

#### 实现步骤：

1. **定义自定义消息**：在 `CResumeManagementSystemView` 中定义一个自定义消息常量。
2. **发送消息给主窗口**：在对话框按钮的响应函数中使用 `PostMessage` 向主窗口发送消息。
3. **转发消息到视图**：在主窗口中捕获消息并将它转发给 `CResumeManagementSystemView`。
4. **处理视图中的消息**：在 `CResumeManagementSystemView` 中添加消息映射以处理接收到的消息。

#### 示例代码：

##### 在 `CResumeManagementSystemView` 中定义自定义消息：

```cpp
// CResumeManagementSystemView.h
#define WM_USER_RESUME_ADDED (WM_USER + 100)
```

##### 在对话框按钮的响应函数中发送消息：

```cpp
void CResumeInfoDlgAdd::OnBnClickedOk()
{
    UpdateData(TRUE);

    // 获取主窗口指针
    CMainFrame* pMainFrame = (CMainFrame*)AfxGetMainWnd();
    if (pMainFrame != NULL)
    {
        // 发送消息给主窗口
        pMainFrame->PostMessage(WM_USER_RESUME_ADDED, 0, 0);
    }

    OnOK(); // 关闭对话框
}
```

##### 在另一个窗口中捕获消息并转发给视图：

```cpp
BEGIN_MESSAGE_MAP(CMainFrame, CFrameWndEx)
    ON_MESSAGE(WM_USER_RESUME_ADDED, &CMainFrame::OnUserResumeAdded)
END_MESSAGE_MAP()

LRESULT CMainFrame::OnUserResumeAdded(WPARAM wParam, LPARAM lParam)
{
    // 获取当前活动视图
    CView* pActiveView = GetActiveView();
    if (pActiveView && dynamic_cast<CResumeManagementSystemView*>(pActiveView))
    {
        // 转发消息给视图
        pActiveView->PostMessage(WM_USER_RESUME_ADDED, wParam, lParam);
    }
    return 0;
}
```

##### 在 `CResumeManagementSystemView` 中处理消息：

```cpp
BEGIN_MESSAGE_MAP(CResumeManagementSystemView, CFormView)
    ON_MESSAGE(WM_USER_RESUME_ADDED, &CResumeManagementSystemView::OnUserResumeAdded)
END_MESSAGE_MAP()

LRESULT CResumeManagementSystemView::OnUserResumeAdded(WPARAM wParam, LPARAM lParam)
{
    // 处理来自对话框的消息
    AfxMessageBox(_T("简历已添加！"));

    // 可以在这里执行其他操作，比如刷新界面等

    return 0;
}
```