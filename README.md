# Ruby Method 講義

在後面 rails 教學的部分，method 的運用扮演著非常重要的角色。當中你可能不需要自己定義一個 method，但卻無法避免需要大量使用其他人所寫的 method。這時候看懂自己在寫些什麼就變成一件非常重要的事。下面整理了一些 method 的基礎還有一些未來你會看到的 method 它們為什麼是這樣寫的範例。

## Method 用處

### 重複使用相同的程式

很多時候會發現自己使用相同的程式碼很多次，這時候就可以將它們合併在一個 method 裡面重複利用。這樣會有以下兩個好處：

* 加速開發，用一個 method 取代多行程式
* 更好維護，如果需要修改，只要改一次就可以讓所有地方都被改道

### 將複雜難懂的功能收進黑盒子

每個人的專業領域不同，不可能有人可以精通所有功能的實作。所以懂的做出某個功能的人可以將程式寫好，包在 method 裡面，讓其他人就算不知道這個功能是怎麼做出來的，也可以達到相同的目的。

就算是自己寫的程式，有時候也需要分類，方便之後來維護。例如控制一個機器人出門可能可以分成走路、開門及關門三個 method，每一個可能都有很複雜的程式。這樣當開門這個步驟出現問題的時候，可以直接去找開門對應到的 method，針對那個部分進行 debug 就好，不用一次看著所有的 code 慢慢尋找。如此可以節省很多時間，也會讓腦袋比較清晰。

## Method 的定義

定義方法：

```ruby
def greet
  puts 'Hello!'
end
```

使用方法：

```ruby
greet
```

## 傳入參數

有參數的 method 定義方法：

```ruby
def say_hi_to(name)
  puts "Hi #{name}"
end
```

使用方法：

```ruby
say_hi_to('alpaca')
```

> ruby 社群在命名的時候，如果是多個單字的組合，習慣採用蛇形命名法（snake case）。這種命名法是用底線連結每個單字，例如剛剛的 say\_hi\_to 就是把 say hi to 用底線連接而成。

### 也可以同時傳入多個參數

定義方法：

```ruby
def greet_three_people(name1, name2, name3)
  puts "Hi #{name1}, #{name2} and #{name#}"
end
```

使用方法：

```ruby
greet_three_people('Alice', 'Bob', 'Tom')
```

### 括號可以省略

在 ruby 中大部分的時候，當你在使用 method 時傳入參數，後面的括號是可以省略的！我們常用的 puts 也是其中一個例子，這也是為什麼我們會可以寫 `puts 'Hello, world!'` 而不用一定要寫 `puts('Hello, world!')`，雖然這樣也可以。

未來常見範例：

```ruby
# 有括號的樣子：link_to('Articles', '/articles')
link_to 'Articles', '/articles'

# 有括號的樣子：redirect_to(article_path(article))
# 這個例子其實是在 method 裡面又放入一個 mothed
redirect_to article_path(article)
```

### 如果最後一個參數是 hash 的話，大括號可以省略

在 rails 裡面的 method 常常最後面都會傳入一整個 hash 讓使用者可以做很多額外的設定。而如果參數的最後一個是一個 hash 的話可以把大括號省略掉。

未來常見範例：

```ruby
# 未省略的樣子：form_for @article, { url: '/articles', remote: true, method: 'post' }
form_for @article, url: '/articles', remote: true, method: 'post'

# 未省略的樣子：Article.create { title: 'test', content: 'this is a test' }
Article.create title: 'test', content: 'this is a test'
```

## 回傳值

Method 除了可以幫你重複執行一些之外，很大一部份也是用來幫你計算某些結果然後傳出來給你使用。例如你可以寫一個 method 幫你計算某個數的三次方：

```ruby
def cubed(x)
  return x * x * x
end
```

使用 `return` 就可以將想要的結果回傳出去，在使用的時候就可以直接接收：

```ruby
cubed_number = cubed(2)
```

這樣就可以把 `cubed_number` 指定為 8。

### Return 可以不用寫？

比較值得注意的是，在 ruby 中所有 method 都一定有回傳值，所以如果你沒有寫 `return` 那 ruby 就會回傳它最後執行的程式的回傳值。例如剛剛的 method 最後運算的東西是 `x * x * x` 所以就算不寫 `return`，三次方的結果也會被自動回傳。所以這樣很多人會直接寫成：

```ruby
def cubed(x)
  x * x * x
end
```

往後也會看到一些這樣的例子，就要注意雖然沒有寫 `return` 但實際上還是有回傳值的，例如：

```ruby
def article_params
  params.require(:article).permit(:title, :content)
end
```

其實就是把 `return` 省略了，原文應該長這樣：

```ruby
def article_params
  return params.require(:article).permit(:title, :content)
end
```



