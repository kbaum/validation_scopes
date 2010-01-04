# Validation Scopes

This gem adds a simple class method `validation_scope` to ActiveRecord::Base.  Inside the block you can define validations that don't apply to the life-cycle of the object (ie. they aren't implicitly checked when the object is saved).

    class Film < ActiveRecord::Base
      validation_scope :warnings do |s|
        s.validates_presence_of :title
      end
    end

The generated scope produces 3 helper methods based on the symbol passed to the validation_scope method.  Continuing the previous example:

    film.warnings      # analagous to film.errors
    => #<ActiveRecord::Errors>

    film.no_warnings?  # analagous to film.valid?
    => true

    film.has_warnings?  # analagous to film.invalid?
    => false


## Implementation

The implementation of this gem is meta-programming heavy and very dependent on the internal structure of ActiveRecord as of Rails 2.3.5.  The goal was to be able to utilize the entire functionality of ActiveRecord validations with a minimum of code.  The resulting code is a bit more magic than I would have liked, but it seems to be working for me so far.  I plan on forward porting to Rails 3 and maybe it will be nicer.

The core of the implemention is a dynamically created proxy_class that has ActiveRecord::Validations included in order to isolate its own copy of @errors.  The proxy class is a DelegateClass which gets initialized to the base ActiveRecord object so that validations can use any of the methods of the base object.



## Copyright

Copyright (c) 2010 Gabe da Silveira. See LICENSE for details.
