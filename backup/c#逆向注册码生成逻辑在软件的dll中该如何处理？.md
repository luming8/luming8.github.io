# c#逆向注册码生成逻辑在软件的dll中

使用内存dump方法

分析软件是32位的还是64位，然后决定使用dnspy32位还是64位的，今天是32位的。

软件分析是从dll加载的，直接附加到进程
![image](https://cdn.jsdelivr.net/gh/luming8/picgo/images/image.png)

从下方模块，找到该进程保存模块为dll文件
![20250225230418](https://cdn.jsdelivr.net/gh/luming8/picgo/images/20250225230418.png)

dnspy重新加载dll文件，右键编辑模块重新设置入口点，不然是打不开的
![20250225230657](https://cdn.jsdelivr.net/gh/luming8/picgo/images/20250225230657.png)

选择好入口点确定保存为exe文件运行即可
![20250225230751](https://cdn.jsdelivr.net/gh/luming8/picgo/images/20250225230751.png)

其中主要代码丢给ai分析一下加个按钮生成注册码，时间加长一点
```Csharp
using System;
using System.ComponentModel;
using System.Drawing;
using System.Windows.Forms;
using DBCTools.RegCode;
using Microsoft.Win32;

namespace DBCTools
{
    public class Form2 : Form
    {
        // ... 原有字段保持不变 ...

        // 新增生成按钮字段
        private Button buttonGenerate;

        public Form2()
        {
            // 构造函数保持不变 ...
        }

        // 新增生成按钮点击事件
        private void buttonGenerate_Click(object sender, EventArgs e)
        {
            if (this.textBox1.Text == "获取机器码失败" || string.IsNullOrEmpty(this.textBox1.Text))
            {
                MessageBox.Show("无法生成注册码：机器码无效");
                return;
            }
            
            string machineCode = this.textBox1.Text;
            DateTime expirationDate = DateTime.Now.AddYears(1000);
            this.textBox2.Text = RegInfo.CreateRegisterCode(machineCode, expirationDate);
        }

        // ... 保留原有的 button1_Click、Dispose 方法 ...

        private void InitializeComponent()
        {
            // 原有初始化代码...
            
            // 新增生成按钮初始化
            this.buttonGenerate = new Button();
            this.buttonGenerate.Font = new Font("宋体", 10.5f);
            this.buttonGenerate.Location = new Point(74, 196);  // 调整位置
            this.buttonGenerate.Name = "buttonGenerate";
            this.buttonGenerate.Size = new Size(100, 29);
            this.buttonGenerate.TabIndex = 6;
            this.buttonGenerate.Text = "生成注册码";
            this.buttonGenerate.UseVisualStyleBackColor = true;
            this.buttonGenerate.Click += new EventHandler(this.buttonGenerate_Click);

            // 调整原确定按钮位置
            this.button1.Location = new Point(200, 196);
            
            // 在窗体控件列表中添加新按钮
            base.Controls.Add(this.buttonGenerate);
            
            // 其余初始化代码保持不变...
        }

        // ... 其余代码保持不变 ...
    }
}

```
主要改动说明：

新增了生成按钮的字段声明 private Button buttonGenerate;

添加了按钮点击事件处理方法 buttonGenerate_Click

在初始化方法中：

创建并配置生成按钮
调整原确定按钮位置
将新按钮添加到窗体控件集合
生成按钮功能：

检查机器码有效性
生成1000年有效期的注册码
自动填充到注册码文本框
界面布局调整：

原确定按钮右移
生成按钮位于左侧
保持整体布局协调
注意：实际部署前需要确保：

控件位置数值根据实际窗体尺寸调整
异常处理机制完善（如加密算法异常）
国际化和文本本地化
界面元素的Tab顺序调整
这个实现允许用户：

自动获取机器码
点击"生成注册码"按钮快速生成
手动验证并保存注册码
保持原有的注册验证逻辑不变


附练习工具 https://lm88.lanzoub.com/i9YPP2ounncf   
逆向成品 https://lm88.lanzoub.com/iUT9C2ouotdi