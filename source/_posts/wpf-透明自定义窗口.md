---
title: WPF 透明自定义窗口
tags:
  - 'C#'
  - WPF
id: .nan
categories:
  - 'C#'
  - 程序设计
date: 2016-01-02 17:07:00
---

![Design-sketch](http://www.heekei.cn/wp-content/uploads/2016/01/Design-sketch.png "Design-sketch")

重写了标题栏，最大化、还原、最小化功能。

突然发现忘记加上程序的标题了，算了，懒得添加了。

MainWindow.xaml代码如下：

MainWindow.xaml.cs代码如下：

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;
    using System.Windows;
    using System.Windows.Controls;
    using System.Windows.Data;
    using System.Windows.Documents;
    using System.Windows.Input;
    using System.Windows.Media;
    using System.Windows.Media.Imaging;
    using System.Windows.Navigation;
    using System.Windows.Shapes;
    using System.Runtime.InteropServices;

    namespace WpfApplication1
    {
        /// 
        /// MainWindow.xaml 的交互逻辑
        /// 
        public partial class MainWindow : Window
        {
            public MainWindow()
            {
                InitializeComponent();
            }
            private bool IsMaximized = false;
            private Rect rcNormal;
            private void TopBorder_MouseLeftButtonDown(object sender, MouseButtonEventArgs e)
            {

                if (e.ClickCount == 1)
                {
                    this.DragMove();
                }
                if (e.ClickCount == 2)
                {
                    MaxOrNormal();
                }
            }

            private void btnClose_Click(object sender, RoutedEventArgs e)
            {
                this.Close();
            }

            private void btnClose_MouseMove(object sender, MouseEventArgs e)
            {
                bgOfClose.Fill = new SolidColorBrush(Colors.Red);
            }

            private void btnClose_MouseLeave(object sender, MouseEventArgs e)
            {
                bgOfClose.Fill = new SolidColorBrush(Colors.Black);
            }

            private void btnMin_MouseMove(object sender, MouseEventArgs e)
            {
                bgOfMin.Fill = new SolidColorBrush(Colors.Gray);
            }

            private void btnMin_MouseLeave(object sender, MouseEventArgs e)
            {
                bgOfMin.Fill = new SolidColorBrush(Colors.Black);
            }

            private void btnMin_Click(object sender, MouseButtonEventArgs e)
            {
                this.WindowState = WindowState.Minimized;
            }

            private void Window_SizeChanged(object sender, SizeChangedEventArgs e)
            {
                if (this.ActualHeight &gt; SystemParameters.WorkArea.Height || this.ActualWidth &gt; SystemParameters.WorkArea.Width)
                {
                    this.WindowState = System.Windows.WindowState.Normal;
                    MaxOrNormal();
                }
                //else
                //{
                //    winBorder.CornerRadius = new CornerRadius(10);
                //    TopBorder.CornerRadius = new CornerRadius(10, 10, 0, 0);
                //}
            }

            private void winBorder_SizeChanged(object sender, SizeChangedEventArgs e)
            {
                TopBorder.Width = this.Width;
            }

            private void Grid_SizeChanged(object sender, SizeChangedEventArgs e)
            {
                TopBorder.Width = this.Width;
            }

            private void btnMax_Click(object sender, MouseButtonEventArgs e)
            {
                MaxOrNormal();

            }

            private void btnMax_MouseMove(object sender, MouseEventArgs e)
            {
                bgOfMax.Fill = new SolidColorBrush(Colors.Gray);
            }

            private void btnMax_MouseLeave(object sender, MouseEventArgs e)
            {
                bgOfMax.Fill = new SolidColorBrush(Colors.Black);
            }

            private void MaxOrNormal() {
                if (IsMaximized)
                {
                    IsMaximized = false;
                    this.Left = rcNormal.Left;
                    this.Top = rcNormal.Top;
                    this.Width = rcNormal.Width;
                    this.Height = rcNormal.Height;
                    this.ResizeMode = ResizeMode.CanResizeWithGrip;
                    winBorder.CornerRadius = new CornerRadius(10);
                    TopBorder.CornerRadius = new CornerRadius(10, 10, 0, 0);
                }
                else
                {
                    IsMaximized = true;
                    rcNormal = new Rect(this.Left, this.Top, this.Width, this.Height);//保存下当前位置与大小
                    Rect rc = SystemParameters.WorkArea;
                    this.Left = 0;
                    this.Top = 0;
                    this.Width = rc.Width;
                    this.Height = rc.Height;
                    this.ResizeMode = ResizeMode.CanResize;
                    winBorder.CornerRadius = new CornerRadius(0);
                    TopBorder.CornerRadius = new CornerRadius(0);
                }
            }
        }
    }