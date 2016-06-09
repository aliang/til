## Using `let` in minitest

We use minitest, and it doesn't have `let`, which leads to a lot of boilerplate setup/teardown code.

Well, minitest is just classes, so you can emulate `let` with a memoizing method.

````ruby
class MyTest < Minitest::Test
  def my_var
    @my_var ||= 1
  end

  test 'it works'
    assert_equal(1, my_var)
  end
end
````

You could even just monkeypatch `Minitest::Test`. This is the gist of what I would do, but I haven't tested it yet.

class Minitest::Test
  def let(var_name, &blk)
    @memo[var_name]
    
    define_method var_name
      @memo[var_name] ||= blk.call
    end
  end
end

And I think there is some module you can include that already has this defined along with some of the other RSpec niceties.

See http://chriskottom.com/blog/2014/10/4-fantastic-ways-to-set-up-state-in-minitest/
