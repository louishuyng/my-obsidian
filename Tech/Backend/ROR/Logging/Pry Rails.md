---
tags: ROR, Backend
---
### show-doc

```ruby
pry(main)> show-doc binding.pry
From: (...)/gems/pry-0.10.4/lib/pry/core_extensions.rb @ line 23:
Owner: Object
Visibility: public
Signature: pry(object=?, hash=?)
Number of lines: 18
Start a Pry REPL on self.
If `self` is a Binding then that will be used to evaluate expressions;
otherwise a new binding will be created.
param [Object] object  the object or binding to pry
                        (__deprecated__, use `object.pry`)
param [Hash] hash  the options hash
example With a binding
   binding.pry
example On any object
  "dummy".pry
example With options
  def my_method
    binding.pry :quiet => true
  end
  my_method()
@see Pry.start

```

### ls

```ruby
pry(main)> our_string = "some TEXT!"
pry(main)> our_string.pry
pry("some TEXT!")>

pry("some TEXT!")> self
=> "some TEXT!"
pry("some TEXT!")> upcase
=> "SOME TEXT!"
pry("some TEXT!")> lowercase
NameError: undefined local variable or method `lowercase' for "some TEXT!":String
from (pry):32:in `__pry__'
pry("some TEXT!")> ls
(...)
Comparable#methods: <  <=  >  >=  between?
Colorize::InstanceMethods#methods: colorize  colorized?  uncolorize
String#methods:
%  classify    gray     mb_chars  rpartition  to_c
*  clear       grayish  next      rstrip      to_d
+  codepoints  green    next      rstrip!     to_date
ActiveSupport::ToJsonWithActiveSupportEncoder#methods: to_json
self.methods: __pry__
(...)
pry("some TEXT!")> ls -G case
String#methods: camelcase casecmp downcase downcase! swapcase  swapcase! titlecase upcase upcase!
pry("some TEXT!")> downcase
=> "some text!"
pry("some TEXT!")> exit
=> nil
pry(main)> self
=> main

```

### cd

```ruby
pry(main)> our_hash = { first: [1, 2, 3], second: "text" }
=> { :first => [1, 2, 3], :second => "text" }
pry(main)> cd our_hash
pry(#<Hash>):1> self
=> { :first => [1, 2, 3], :second => "text" }
pry(#<Hash>):1> cd self[:first]
pry(#<Array>):2> self
=> [1, 2, 3]
pry(#<Array>):2> nesting
Nesting status:
--
0. main (Pry top level)
1. #<Hash>
2. #<Array>
pry(#<Array>):2> jump-to 1
pry(#<Hash>):1> self
=> { :first => [1, 2, 3], :second => "text" }
pry(#<Hash>):1> cd self[:first]
pry(#<Array>):2> nesting
Nesting status:
--
0. main (Pry top level)
1. #<Hash>
2. #<Array>
pry(#<Array>):2> cd ..
pry(#<Hash>):1> self
=> { :first => [1, 2, 3], :second => "text" }
pry(#<Hash>):1> nesting
Nesting status:
--
0. main (Pry top level)
1. #<Hash>
pry(#<Hash>):1> cd /
pry(main)>

```

### show-source

```ruby
pry(main)> show-source Array
From: (...)/lib/active_support/core_ext/array/access.rb @ line 1:
Class name: Array
Number of monkeypatches: 13. Use the `-a` option to display all available monkeypatches
Number of lines: 64
class Array
  # Returns the tail of the array from +position+.
(...)
pry(main)> show-source Array#second
From: (...)/lib/active_support/core_ext/array/access.rb @ line 33:
Owner: Array
Visibility: public
Number of lines: 3
def second
  self[1]
end

```

### play
```ruby
From: (...)/application_controller.rb @ line 7 ApplicationController#controller_method: 5: def controller_method 6: binding.pry => 7: puts "lots of code with" 8: puts "nestings and loops..." 9: end pry(#<ApplicationController>)> play -l 7..8 lots of code with nestings and loops...
```