= slug

Slug provides simple, straightforward slugging for your ActiveRecord models.

Slug is based on code from Norman Clarke's fantastic {friendly_id}[http://friendly-id.rubyforge.org] project and Nick Zadrozny's {friendly_identifier}[http://code.google.com/p/friendly-identifier/].

What's different:

* Unlike friendly_id, slugs are stored directly in your model's table.  friendly_id stores its data in a separate sluggable table, which enables cool things like slug versioning--but forces yet another join when trying to do complex find_by_slugs.
* Like friendly_id, diacritics (accented characters) are stripped from slug strings.
* Most importantly, there are just two options: source column (for example, headline) and an optional slug column (if for some reason you don't want your slugs stored in a column called slug)

== INSTALLATION

  gem 'slug',                      :git => 'git://github.com/willdrew/slug.git'

to your rails project.

Note that this version of slug is only compatible with Rails/active_support/active_record 3.x+.  Rails 2 applications should use slug 0.5.3.

== USAGE

It's up to you to set up the appropriate column in your model.  By default, Slug saves the slug to a column called 'slug', so in most cases you'll just want to add

  t.string :slug

to your create migration.

I'd also suggest adding a unique index on the slug field in your migration

  add_index :model_name, :slug, :unique => true

Though Slug uses validates_uniqueness_of to ensue the uniqueness of your slug, two concurrent INSERTs could conceivably try to set the same slug.  Unlikely, but it's worth adding a unique index as a backstop.

Once your table is migrated, just add

  slug :source_field

to your ActiveRecord model.  <tt>:source_field</tt> is the column you'd like to base the slug on--for example, it might be <tt>:headline</tt>.

If you want to save the slug in a database column that isn't called <tt>slug</tt>, just pass the <tt>:column</tt> option. For example

  slug :headline, :column => :web_slug

would generate a slug based on <tt>headline</tt> and save it to <tt>web_slug</tt>.

The source column isn't limited to just database attributes--you can specify any instance method.  This is handy if you need special formatting on your source column before it's slugged, or if you want to base the slug on several attributes.

For example:

  class Article < ActiveRecord::Base
    slug :title_for_slug

    def title_for_slug
      "#{headline}-#{publication_date.year}-#{publication_date.month}"
    end
  end
end

would use headline-pub year-pub month as the slug source.

From here, you can work with your slug as if it were a normal column--<tt>find_by_slug</tt> and named scopes will work as they do for any other column.

A few notes:
* Slug validates presence and uniqueness of the slug column.  If you pass something that isn't sluggable as the source (for example, say you set the headline to '---'), a validation error will be set.
* Slug doesn't update the slug if the source column changes.  If you really need to regenerate the slug, call <tt>@model.set_slug</tt> before save.
* If a slug already exists, Slug will automatically append a '-n' suffix to your slug to make it unique.  The second instance of a slug is '-1'.
* If you don't like the slug formatting or the accented character stripping doesn't work for you, it's easy to override Slug's formatting functions. Check the source for details.

== AUTHOR

Ben Koski, ben.koski@gmail.com

with generous contributions from:
Derek Willis, @derekwillis
Rob Lewis, @kohder (forked)
Will Drew, @willdrew
