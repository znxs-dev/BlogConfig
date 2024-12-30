---
title: MFC 图像上传显示处理
date: 2024-12-30 13:52:08
description: MFC 图像上传显示处理，用于MFC VC++ 图片系列功能实现
tags:
  - MFC
  - VC++
categories:
cover: https://img.znxs.vip/study/202111090821216933.png
top_img: https://img.znxs.vip/study/202111090821216933.png
---

1

## MFC图像处理

### 上传图片

这里上传图片之后直接显示了，调用了自定义的显示图片的方法 [ShowImage](#显示图片)

```cpp
// 添加图片 （打开文件选择器）
void CImageViewView::OnBnClickedButtonAddImg()
{
	// TODO: 在此添加控件通知处理程序代码
	 //选择图片
	CFileDialog fileDlg(TRUE, _T("png"), NULL, 0, _T("image Files(*.bmp; *.jpg;*.png)|*.JPG;*.PNG;*.BMP|All Files (*.*) |*.*||"), this);
	//打开文件选择窗体
	if (fileDlg.DoModal() == IDCANCEL) return; //如果点击“取消”按钮，直接退出
	//获取图片路径（包含名称）
	CString strFilePath = fileDlg.GetPathName();//既有路径又有文件名，如D:/lena.jpg
	CString strFileName = fileDlg.GetFileName();//只获取文件名，如lena.jpg
	//判断路径不为空
	OutputDebugString(strFilePath+"文件路径为：【】【】【】");
	if (strFilePath == _T(""))
	{
		return;
	}

	//保存文件到指定目录下
	if (!CopyFileToDirectory(strFilePath, m_strDestinationDir)) {
		return;
	}

	// 显示图片
	ShowImage(m_strDestinationDir + "\\" + strFileName);

}
```

### 显示图片

通过画布的方式显示

```cpp
// 显示图片（根据路径显示图片）
void CImageViewView::ShowImage(CString strFilePath) {
	//判断路径是否为空
	if (strFilePath == _T(""))
	{
		return;
	}
	CImage image; //使用图片类，已放到头文件中（全局变量）
	if (!image.IsNull())
		image.Destroy(); //如果已有，先清空。否则重新加载会报错。
	//使用CImage的Load、Draw函数显示图像
	image.Load(strFilePath);

	//int x = image.GetWidth(); //学习下CImage的属性，无其他意义
	//int y = image.GetHeight();

	//获取控件的矩形
	CRect rectControl;  //控件矩形对象
	CWnd* pWnd = GetDlgItem(IDC_STATIC_IMG_VIEW); //Picture Control的ID为IDC_IMAGE
	pWnd->GetClientRect(&rectControl);

	// 清空画布
	if (pWnd != NULL)
	{
		CRect rect;
		pWnd->GetClientRect(&rect); // 获取控件的客户区矩形
		CDC* pDC = pWnd->GetDC();   // 获取设备上下文
		if (pDC != NULL)
		{
			pDC->FillSolidRect(&rect, RGB(255, 255, 255)); // 用白色填充，可以根据需要改变颜色
			pWnd->ReleaseDC(pDC); // 释放DC
		}
	}

	//以控件为画布，在其上画图
	CDC* pDc = GetDlgItem(IDC_STATIC_IMG_VIEW)->GetDC();
	SetStretchBltMode(pDc->m_hDC, STRETCH_HALFTONE);//绘图前必须调用此函数（设置缩放模式），否则失真严重

	//画图（以下两种方法都可）
	//image.StretchBlt(pDc->m_hDC, rectPicture, SRCCOPY); //将图片绘制到Picture控件表示的矩形区域
	image.Draw(pDc->m_hDC, rectControl);                //将图片绘制到Picture控件表示的矩形区域

	image.Destroy();
	pWnd->ReleaseDC(pDc);
}
```



### 复制文件

> 复制图片到指定路径下，用于更好的显示图片

```cpp
// 保存图片（保存指定文件到指定路径下）
BOOL CImageViewView::CopyFileToDirectory(CString strSourceFilePath, CString strDestinationDir)
{
	// 确保目标目录存在。如果不存在，则创建它。
	if (!PathIsDirectory(strDestinationDir))
	{
		CreateDirectory(strDestinationDir, NULL);
	}

	// 获取文件名（不带路径）
	CString strFileName = PathFindFileName(strSourceFilePath);

	// 构建完整的目标路径
	CString strDestFilePath = strDestinationDir + _T("\\") + strFileName;

	// 复制文件
	if (!CopyFile(strSourceFilePath, strDestFilePath, FALSE))  // TRUE 表示如果目标文件已存在则覆盖
	{
		DWORD dwError = GetLastError();
		CString strError;
		strError.Format(_T("文件上传失败，错误代码：%d"), dwError);
		AfxMessageBox(strError);
		return FALSE;
	}
	return TRUE;
}
```

