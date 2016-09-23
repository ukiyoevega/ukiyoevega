---
layout: post
title:  markdown grammar
categories: jekyll
description: description here.
---

Simple is a beautiful but functional jekyll theme. The font-type setting looks really good when writers use CJK mixed with English.

First of all, let's have a glance at the basic styles: [link](http://github.com/wild-flame/jekyll-simple), **strong**, *italic*, <del>deletion</del>, <ins>insertion</ins>.

<!--description-->

### Headers:

# Header 1

## Header 2

### Header 3

#### Header 4

##### Header 5

###### Header 6

### Lists:

- list item 1
- list item 2
- list item 3

1. list item 1
2. list item 2
3. list item 3

### Blockquote:

> Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.

### [BASSCSS](http://www.basscss.com/) colors:

- <span class="black">black</span>
- <span class="gray">gray</span>
- <span class="silver">silver</span>
- <span class="white">white</span>
- <span class="aqua">aqua</span>
- <span class="blue">blue</span>
- <span class="navy">navy</span>
- <span class="teal">teal</span>
- <span class="green">green</span>
- <span class="olive">olive</span>
- <span class="lime">lime</span>
- <span class="yellow">yellow</span>
- <span class="orange">orange</span>
- <span class="red">red</span>
- <span class="fuchsia">fuchsia</span>
- <span class="purple">purple</span>
- <span class="maroon">maroon</span>

### Horizontal rule:

-----------------------

### Image:

![]({{site.baseurl}}/assets/img/image.jpg)

### Table:

<table>
	<thead>
		<tr>
			<th>Name</th>
			<th>Age</th>
			<th>Fruit</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td>Alex</td>
			<td>22</td>
			<td>Apple</td>
		</tr>
		<tr>
			<td>Bran</td>
			<td>20</td>
			<td>Orange</td>
		</tr>
		<tr>
			<td>Mike</td>
			<td>21</td>
			<td>Waltermelon</td>
		</tr>
	</tbody>
</table>

### Code snippet

```javascript
// index.js
var arr = [1, 2, 3, 4, 5];
var b = arr.map(x => x * x);
console.log(b);

function foo(){
	console.log('foo');
}

```
{% highlight ruby %}  
def print_hi(name)
  puts "Hi, #{name}"
end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.
{% endhighlight %}

To add new posts, simply add a file in the `_posts` directory that follows the convention `YYYY-MM-DD-name-of-post.ext` and includes the necessary front matter. Take a look at the source for this post to get an idea about how it works.

Check out the [Jekyll docs][jekyll-docs] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll Talk][jekyll-talk].

[jekyll-docs]: http://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
