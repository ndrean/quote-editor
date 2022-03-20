# Learning Rails

## Source

<https://www.hotrails.dev/turbo-rails/css-ruby-on-rails>

## Generate model

```bash
rails generate model Quote name:string
rails generate controller Quotes
bundle add simple_form
rails generate simple_form:install
```

## Setup test

### Use `headless_chrome` for testing

Instead of `chrome`, use `:headless_chrome` to avoid the browser to open a page

```rb
# test/application_system_test_case.rb

class ApplicationSystemTestCase < ActionDispatch::SystemTestCase
  # Change :chrome with :headless_chrome
  driven_by :selenium, using: :headless_chrome, screen_size: [1400, 1400]
end
```

### Seeding

Two sources to create development data:

- from "/test/fixtures" (for the tests): we can run:
`rails db:fixtures:load`

- from "/db/seeds.rb" (for the dev mode): we can run `rails db.seed`.

Adding `system("bin/rails db:fixtures:load")` in "/db/seeds.rb" tells Rails that the two commands are equivalent. Running the `bin/rails db:seed` command is now equivalent to removing all the quotes and loading fixtures as development data. Every time we need to reset a clean development data, we can run the `rails db:seed` command.

### CSS

Our "app/assets/stylesheets/" folder will contain four elements:

The application.sass.scss manifest file to import all our styles.

- A "mixins/" folder where we'll add Sass mixins.
- A "/config/" folder where we'll add our variables and global styles.
- A "/components/" folder where we'll add our components.
- A "/layouts/" folder where we'll add our layouts.

We need a `.visually-hidden` component to hide the input label and not simply remove it from the DOM or use display: none;. For accessibility purposes, all form inputs should have labels that can be interpreted by screen readers even if they are not visible on the web page. This is why most applications use a .visually-hidden component.

The app is mobile first, so we define our CSS for the smallest screen sizes (mobiles), then add overrides for larger screen sizes. The breakpoint corresponds to as "tabletAndUp" is easier to read than 50rem.

```css
.my-component-1 {
  // The CSS for mobile

  @include media(tabletAndUp) {
    // The CSS for screens bigger than tablets
  }
}
```
## Turbo Frames

The `<turbo-frame>` HTML tag does not exist in the HTML language. It is a custom element that was created in the Turbo JavaScript library. It intercepts form submissions and clicks on links within the frame, making those frames independent pieces of your web page.

- R1: When clicking on a link within a Turbo Frame, Turbo expects a frame of the same id on the target page. It will then replace the Frame's content on the source page with the Frame's content on the target page.

- R2: When clicking on a link within a Turbo Frame, if there is no Turbo Frame with the same id on the target page, the frame disappears, and the error Response has no matching `<turbo-frame id="name_of_the_frame">` element is logged in the console.

- R3: A link can target another frame than the one it is directly nested in thanks to the `data-turbo-frame` data attribute.

### Syntatic sugar: `dom_id` helpers

```html
<%= turbo_frame_tag "quote_#{quote.id}" do %>...<% end %>
equivalent to:
<%= turbo_frame_tag dom_id(quote) do %>...<% end %>
```