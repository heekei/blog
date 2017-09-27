---
title: 'Call a C# Method From JavaScript Hosted in a WebBrowser'
id: .nan
categories:
  - 'C#'
  - web开发
  - 程序设计
date: 2015-12-20 11:28:57
tags:
---

> This demonstrates how you can call a C# method in a Windows Forms application from JavaScript that is hosted in a webpage inside a WebBrowser control on your form.

This sample demonstrates how to call C# from JavaScript. It also shows that parameters can be passed to C# methods.

First, create a Windows Forms application. Then, add a WebBrowser control to your form. Then modify the code for the form so it looks like this:
<!--more-->

    namespace WindowsFormsApplication6
    {
        // This first namespace is required for the ComVisible attribute used on the ScriptManager class.
        using System.Runtime.InteropServices;
        using System.Windows.Forms;

        // This is your form.
        public partial class Form1 : Form
        {
            // This nested class must be ComVisible for the JavaScript to be able to call it.
            [ComVisible(true)]
            public class ScriptManager
            {
                // Variable to store the form of type Form1.
                private Form1 mForm;

                // Constructor.
                public ScriptManager(Form1 form)
                {
                    // Save the form so it can be referenced later.
                    mForm = form;
                }

                // This method can be called from JavaScript.
                public void MethodToCallFromScript()
                {
                    // Call a method on the form.
                    mForm.DoSomething();
                }

                // This method can also be called from JavaScript.
                public void AnotherMethod(string message)
                {
                    MessageBox.Show(message);
                }
            }

            // This method will be called by the other method (MethodToCallFromScript) that gets called by JavaScript.
            public void DoSomething()
            {
                // Indicate success.
                MessageBox.Show("It worked!");
            }

            // Constructor.
            public Form1()
            {
                // Boilerplate code.
                InitializeComponent();

                // Set the WebBrowser to use an instance of the ScriptManager to handle method calls to C#.
                webBrowser1.ObjectForScripting = new ScriptManager(this);

                // Create the webpage.
                webBrowser1.DocumentText = @"&lt;html&gt;
                    &lt;head&gt;
                        &lt;title&gt;Test&lt;/title&gt;
                    &lt;/head&gt;
                    &lt;body&gt;
                    &lt;input type=""button"" value=""Go!"" onclick=""window.external.MethodToCallFromScript();"" /&gt;
                        &lt;br /&gt;
                        &lt;input type=""button"" value=""Go Again!"" onclick=""window.external.AnotherMethod('Hello');"" /&gt;
                    &lt;/body&gt;
                    &lt;/html&gt;";
            }
        }
    }

Note that your application may be part of a namespace other than WindowsFormsApplication6, but the rest of the code should work if you follow the above instructions explicitly. I created this tip/trick because somebody asked me a question and they didn't understand this sample that I sent them to. This tip/trick makes the sample more understandable by fixing the two bugs I spotted, adding the using statements that weren't mentioned, and by heavily commenting the code. Hopefully the rest of you will find this of use as well.

Source:[http://www.codeproject.com/Tips/130267/Call-a-C-Method-From-JavaScript-Hosted-in-a-WebBro](http://www.codeproject.com/Tips/130267/Call-a-C-Method-From-JavaScript-Hosted-in-a-WebBro)