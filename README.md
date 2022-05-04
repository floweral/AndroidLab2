# AndroidLab2
# 创建第一个Android Kotlin应用

## 一、创建Kotlin应用的工程

主要是在建立工程的时候选择Basic Activity，并选择Kotlin语言，再使用Gradle进行系统构建。

![项目框架视图](https://github.com/floweral/images/tree/lab2/p1项目架构视图.png)

## 二、实现FirstFragment页面

##### 1、向页面中添加按钮，设置相应的约束以及布局

![按钮视图](https://github.com/floweral/images/tree/lab2/p2按钮视图.png)

##### 2、更改组件文本，修改Next按钮

##### 3、添加颜色资源，设置组件背景色，再次设置组件布局

代码如下：

```XML
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".FirstFragment"
    android:background="@color/screenBackground">

    <TextView
        android:id="@+id/textview_first"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:fontFamily="sans-serif-condensed"
        android:text="0"
        android:textAppearance="@style/TextAppearance.AppCompat.Display2"
        android:textColor="@color/white"
        android:textSize="72sp"
        android:textStyle="bold"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

    <Button
        android:id="@+id/random_button"
        android:layout_width="99dp"
        android:layout_height="39dp"
        android:layout_marginEnd="24dp"
        android:background="@color/buttonBackground"
        android:text="@string/random_button_text"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/textview_first" />


    <Button
        android:id="@+id/toast_button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginStart="24dp"
        android:background="@color/buttonBackground"
        android:text="@string/toast_button_text"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/textview_first"></Button>

    <Button
        android:id="@+id/count_button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/count_button_text"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toStartOf="@+id/random_button"
        app:layout_constraintHorizontal_bias="0.536"
        app:layout_constraintStart_toEndOf="@+id/toast_button"
        app:layout_constraintTop_toBottomOf="@+id/textview_first"
        android:background="@color/buttonBackground"/>
</androidx.constraintlayout.widget.ConstraintLayout>
```

运行结果如下：

![第一个页面运行结果](https://github.com/floweral/images/tree/lab2/p3第一个页面运行结果.png)

##### 4、设置代码自动补全

![代码自动补全1](https://github.com/floweral/images/tree/lab2/p4代码自动补全1.png)

![代码自动补全2](https://github.com/floweral/images/tree/lab2/p5代码自动补全2.png)

##### 5、TOAST按钮添加一个toast消息

在FirstFragment.kt中设置

```kotlin
view.findViewById<Button>(R.id.toast_button).setOnClickListener {
    // create a Toast with some text, to appear for a short time
    val myToast = Toast.makeText(context, "Hello Toast!", Toast.LENGTH_LONG)
    // show the Toast
    myToast.show()
}
```

##### 6、Count按钮加值功能实现

```kotlin
view.findViewById<Button>(R.id.count_button).setOnClickListener {
    countMe(view)
}
```

```kotlin
private fun countMe(view: View) {
    // Get the text view
    val showCountTextView = view.findViewById<TextView>(R.id.textview_first)

    // Get the value of the text view.
    val countString = showCountTextView.text.toString()

    // Convert value to a number and increment it
    var count = countString.toInt()
    count++

    // Display the new value in the text view.
    showCountTextView.text = count.toString()
}
```

代码实现如下：

```kotlin
class FirstFragment : Fragment() {

    private var _binding: FragmentFirstBinding? = null

    // This property is only valid between onCreateView and
    // onDestroyView.
    private val binding get() = _binding!!

    override fun onCreateView(
        inflater: LayoutInflater, container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View? {

        _binding = FragmentFirstBinding.inflate(inflater, container, false)
        return binding.root

    }

    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        super.onViewCreated(view, savedInstanceState)

        binding.randomButton.setOnClickListener {
//            findNavController().navigate(R.id.action_FirstFragment_to_SecondFragment)
            val showCountTextView = view.findViewById<TextView>(R.id.textview_first)
            val currentCount = showCountTextView.text.toString().toInt()
            val action = FirstFragmentDirections.actionFirstFragmentToSecondFragment(currentCount)
            findNavController().navigate(action)
        }
        // find the toast_button by its ID and set a click listener
        view.findViewById<Button>(R.id.toast_button).setOnClickListener {
            // create a Toast with some text, to appear for a short time
            val myToast = Toast.makeText(context, "Hello Toast!", Toast.LENGTH_LONG)
            // show the Toast
            myToast.show()
        }
        view.findViewById<Button>(R.id.count_button).setOnClickListener {
            countMe(view)
        }
    }

    private fun countMe(view: View) {
        // Get the text view
        val showCountTextView = view.findViewById<TextView>(R.id.textview_first)

        // Get the value of the text view.
        val countString = showCountTextView.text.toString()

        // Convert value to a number and increment it
        var count = countString.toInt()
        count++

        // Display the new value in the text view.
        showCountTextView.text = count.toString()
    }

    override fun onDestroyView() {
        super.onDestroyView()
        _binding = null
    }
```

## 三、实现SecondFragment页面

##### 1、添加文本框，进行相应的设置

- 去掉TextView和Button之间的约束
- 设置新的TextView的id为 @+id/textview_random

- 添加新的TextView，并设置相应约束

- 设置新TextView的字体颜色、字体大小、字体样式、显示文字

- 设置垂直偏移量**layout_constraintVertical_bias**为0.45

- ```kotlin
  <TextView
      android:id="@+id/textview_random"
      android:layout_width="wrap_content"
      android:layout_height="wrap_content"
      android:text="R"
      android:textColor="@android:color/white"
      android:textSize="72sp"
      android:textStyle="bold"
      app:layout_constraintBottom_toTopOf="@+id/button_second"
      app:layout_constraintEnd_toEndOf="parent"
      app:layout_constraintStart_toStartOf="parent"
      app:layout_constraintTop_toBottomOf="@+id/textview_header"
      app:layout_constraintVertical_bias="0.45" />
  ```

##### 2、添加颜色资源，更改背景色

更改SecondFragment页面的背景颜色

##### 3、设置按钮布局

更改按钮样式以及布局

##### 4、检查导航图（跳转关系）

![导航图](https://github.com/floweral/images/tree/lab2/p6导航图.png)

##### 5、启用SageArgs（页面间传值）

- 在build.gradle(project)中的depencies添加

  ```
  def nav_version = "2.3.0-alpha02"
  classpath "androidx.navigation:navigation-safe-args-gradle-plugin:$nav_version"
  ```

- 在build.gradle(module)中的plugins中添加

  ```
  id 'androidx.navigation.safeargs'
  ```

- Make project

##### 6、添加传值参数

在SecondFragment页面添加页面传值参数

![页面传值参数](https://github.com/floweral/images/tree/lab2/p7传值参数设置.png)

##### 7、设置发送的数据的代码

实现代码如下：

```kotlin
package com.example.experiment2

import android.os.Bundle
import androidx.fragment.app.Fragment
import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
import android.widget.TextView
import androidx.navigation.fragment.findNavController
import com.example.experiment2.databinding.FragmentSecondBinding
import androidx.navigation.fragment.navArgs

/**
 * A simple [Fragment] subclass as the second destination in the navigation.
 */
class SecondFragment : Fragment() {

    private var _binding: FragmentSecondBinding? = null

    // This property is only valid between onCreateView and
    // onDestroyView.
    private val binding get() = _binding!!

    override fun onCreateView(
        inflater: LayoutInflater, container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View? {

        _binding = FragmentSecondBinding.inflate(inflater, container, false)
        return binding.root

    }

    val args: SecondFragmentArgs by navArgs()
    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        super.onViewCreated(view, savedInstanceState)
        val count = args.myArg
        val countText = getString(R.string.random_heading, count)
        view.findViewById<TextView>(R.id.textview_header).text = countText
        val random = java.util.Random()
        var randomNumber = 0
        if (count > 0) {
            randomNumber = random.nextInt(count + 1)
        }
        view.findViewById<TextView>(R.id.textview_random).text = randomNumber.toString()


        binding.buttonSecond.setOnClickListener {
            findNavController().navigate(R.id.action_SecondFragment_to_FirstFragment)
        }
    }

    override fun onDestroyView() {
        super.onDestroyView()
        _binding = null
    }
}
```

![页面二运行结果](https://github.com/floweral/images/tree/lab2/p2页面二运行结果.png)
