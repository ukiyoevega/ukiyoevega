---
layout: post
title: 不同类型Segue和UnwindSegue的使用情况
tags:
  - storyboard
  - iOS
---

顾名思义，unwindsegue是对segue的撤回，一开始我还以为它只对特定类型的segue有效，结果逐个试验后才发现所有类型的segue(show, show detail, present modally, present as popover, custom, push, modal)都可以对应的用上unwind segue。其中push,modal自iOS8起就被弃用了，而popover在iOS9之前只能用在iPad上，iOS9以后iPhone才开始起用~

方便起见，下面用us代替unwindsegue。

us必须配合@IBAction使用，也就是说，在用到us的view controller中必须定义并实现一个@IBAciton类型的方法。实际使用过程中犯了一个很低级的错误，一开始我的部分代码是这样的:

```
class FirstViewController: UIViewController { 
}
class SecondViewController: UIViewController { 
    @IBAction func gobackToFirst(segue: UIStoryboardSegue) {
    }  
}
class ThirdViewController: UIViewController {  
    @IBAction func gobackToSecond(segue: UIStoryboardSegue) {
    }   
}
```
我把关于us方法的定义放在了us的sourceViewController...也就是调用它的视图控制器，实际上定义应该放在destinationViewController，也就是us应该返回到的那个视图控制器。改正后的代码应该是这样的:

```
class FirstViewController: UIViewController { 
    @IBAction func gobackToFirst(segue: UIStoryboardSegue) {
    }  
}
class ThirdViewController: UIViewController {  
    @IBAction func gobackToSecond(segue: UIStoryboardSegue) {
    }   
}
class ThirdViewController: UIViewController {   
}
```

晕...

---

上面提及的unwindsegue测试结果，分别有入口UIViewController植入和未植入UINavigationController的情况，同时记录了跳转动画的类型。

<table>
	<thead>
		<tr>
			<th>Segue</th>
			<th>植入了NavigationController</th>
			<th>没植入NavigationController</th>
			<th>UnwindSegue</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td>show</td>
			<td>生成带返回按钮的NavitaionBar</td>
			<th>下方弹出</th>
			<th>可用</th>
		</tr>
		<tr>
			<td>show detail</td>
			<td>无NavitaionBar</td>
			<th>下方弹出</th>
			<th>可用</th>
		</tr>
		<tr>
			<td>present modally</td>
			<td>无NavitaionBar</td>
			<th>下方弹出</th>
			<th>可用</th>
		</tr>
		<tr>
			<td>present as popover</td>
			<td>无NavitaionBar</td>
			<th>下方弹出</th>
			<th>可用</th>
		</tr>
		<tr>
			<td>push</td>
			<td>生成带返回按钮的NavitaionBar</td>
			<th>程序异常</th>
			<th>-</th>
		</tr>
		<tr>
			<td>modal</td>
			<td>无NavitaionBar</td>
			<th>下方弹出</th>
			<th>可用</th>
		</tr>
	</tbody>
</table>

<ins>此外通过测试还发现一个情况，就是</ins>

1-2-1

1-2-3

1-3-2F

1-3-1

2-3-1


