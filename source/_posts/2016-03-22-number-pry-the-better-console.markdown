---
layout: post
title: "#Pry - the better console"
date: 2016-03-22 17:56:10 +0100
comments: true
categories:
---

##What is pry?

Pry is ruby console (irb) alternative, with syntax highlighting, runtime invocation and source code browsing.

<!-- more -->
to install it simply type:
```
gem install pry
```

now you can use it by typing ``pry`` anywhere

##Why it is so powerful?

type: ``ls`` to show current class (default is main) methods variables type:``cd`` to change object.

Just like navigating thru directories in Unix/Linux machine

For example:

```
➜ pry
[1] pry(main)> cd String
[2] pry(String):1> ls
Object.methods: yaml_tag
String.methods: try_convert
String#methods:
  %    ===          byteslice    chop        delete          each_line       freeze    insert   match      reverse     scrub        split        succ       to_r    unicode_normalize
  *    =~           capitalize   chop!       delete!         empty?          getbyte   inspect  next       reverse!    scrub!       squeeze      succ!      to_s    unicode_normalize!
  +    []           capitalize!  chr         downcase        encode          gsub      intern   next!      rindex      setbyte      squeeze!     sum        to_str  unicode_normalized?
  +@   []=          casecmp      clear       downcase!       encode!         gsub!     length   oct        rjust       shellescape  start_with?  swapcase   to_sym  unpack
  -@   ascii_only?  center       codepoints  dump            encoding        hash      lines    ord        rpartition  shellsplit   strip        swapcase!  tr      upcase
  <<   b            chars        concat      each_byte       end_with?       hex       ljust    partition  rstrip      size         strip!       to_c       tr!     upcase!
  <=>  bytes        chomp        count       each_char       eql?            include?  lstrip   prepend    rstrip!     slice        sub          to_f       tr_s    upto
  ==   bytesize     chomp!       crypt       each_codepoint  force_encoding  index     lstrip!  replace    scan        slice!       sub!         to_i       tr_s!   valid_encoding?
locals: _  __  _dir_  _ex_  _file_  _in_  _out_  _pry_
```

Unless you forgot:
```
[8] pry(String):1> whereami
Inside String.
```

Now go back

```
[9] pry(String):1> cd ..
[10] pry(main)>
```

Write some class implementation

```
[10] pry(main)> class PryTest
[10] pry(main)*
[10] pry(main)*   def hello
[10] pry(main)*     #simple method that returns hello world
[10] pry(main)*     "hello world"
[10] pry(main)*   end
[10] pry(main)*
[10] pry(main)* end
```

cd into it

```
[12] pry(main)> cd PryTest
[13] pry(PryTest):1>
```

and show-source

```
[16] pry(PryTest):1> show-source

From: (pry) @ line 2:
Class name: PryTest
Number of lines: 8

class PryTest

  def hello
    #simple method that returns hello world
    "hello world"
  end

end
```

We created class at runtime!!

Want more?

install pry-doc

```
gem install pry-doc
```

and

```
pry

[1] pry(main)> cd String
[2] pry(String):1> show-method upcase

From: string.c (C Method):
Owner: String
Visibility: public
Number of lines: 7

static VALUE
rb_str_upcase(VALUE str)
{
    str = rb_str_dup(str);
    rb_str_upcase_bang(str);
    return str;
}
```
Native string method, so ruby is written in C but you should know that already :)

##Easy of reading

Pry has built in code formatting and auto indent, so you always get nice looking output.

it uses `less` paginator, so you can scroll thru big amount of output.

##Executing methods

This feature is obvious, when you are in console you can call invoke any ruby methods.

```
[7] pry(main)> "Test string"
=> "Test string"

# _ stand for previus result "Test String" in this case
[8] pry(main)> _.reverse
=> "gnirts tseT"
[9] pry(main)> _.upcase
=> "GNIRTS TSET"
[10] pry(main)> _.reverse
=> "TEST STRING"
[11] pry(main)> _.downcase
=> "test string"
[12] pry(main)> _.capitalize
=> "Test string"
[13] pry(main)>
```

##Wait for more

There are much more features and plugins in pry cli tool, I will try to cover some of them in next posts.

Happy coding !!

---------------------

Read more at:
http://pryrepl.org
