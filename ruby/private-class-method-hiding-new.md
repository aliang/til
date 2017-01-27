You can use `private_class_method` to make class methods private. Note that it
is not a keyword, you have to call it for each method you want to make
private.

See [`private_class_method` docs][private_class_method_docs].

```ruby
# Works as expected
class A
  private_class_method \
  def self.a
    puts "a"
  end
end
# => NoMethodError: private method `a' called for A:Class
# =>    from (irb):7
# =>    from /Users/alvin/.rbenv/versions/2.3.1/bin/irb:11:in `<main>'

# Doesn't work as expected
class A
  private_class_method

  def self.a
    42
  end
end
A.a
# => 42
```

[private_class_method_docs]: http://ruby-doc.org/core-2.3.3/Module.html#method-i-private_class_method
