Given

```ruby
class Author < ActiveRecord::Base
  has_many :books
end
class Book < ActiveRecord::Base
  belongs_to :author
  scope :available, ->{ where(available: true) }
end
```

you can do

```ruby
Author.joins(:books).where("books.available = ?", true)
```

but even better, you can reuse the scope using `#merge`

```ruby
Author.joins(:books).merge(Book.available)
```
