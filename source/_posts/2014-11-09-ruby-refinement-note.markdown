---
layout: post
title: "Ruby refinement note"
date: 2014-11-09 12:46:11 +0800
comments: true
categories: 
---

Refinement is introduced in ruby 2.0, no longer a experiment feature in ruby 2.1, but still not very stable. The idea of refinement is to limit the monkeying patch scope.

For example, suppose we define a refinement:
  
``` ruby
    module ACoolFeature
      refine(String) do
        def is_it_cool?
          puts "YES"
        end
      end
    end
```

Here we define a refinement to String class, to use the refinement we need to call *using* inside a class.

``` ruby
    class Person
      using(ACoolFeature)
      
      def hello
        "test".is_it_cool?
      end
    end
```

After call using in Person class, the method is_it_cool? is available for string inside this scope.

``` ruby
    Person.new.hello
    >     Person.new.hello
    YES
```

That's it.
