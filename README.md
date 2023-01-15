# Practicing React testing library

https://testing-library.com/docs/react-testing-library/intro/

### Overview of newly acquired knowledge

Tool that shows a web to facilitate to locate elements.
Remember to add a border if you cant select the element.

    screen.logTestingPlaygroundURL()

Example:
[Testing Playground](https://testing-playground.com/#markup=DwEwlgbgfMBmD2AnAtgAgMYBsCGBnXAvAEQCuuApogLQIpEzjTA4BG5mqtxAdtsuUQw58xMpRpJkAfSmt29Xv2AB6OZhhhuABxIAXVGBA8+AoXkKkK1WtKmaduwYvLH+giNkw)

Within method that helps you find inside a specific URL

    within:
    const rows = within(screen.getByTestId('users')).getAllByRole('row');

A nice way to renderComponents on the top, not using beforeEach (with is not recommended)

    function renderComponent() {
        const users = [
          { name: 'jane', email: 'jane@jane.com' },
          { name: 'sam', email: 'sam@sam.com' },
        ];
        render(<UserList users={users} />);
        return {
          users,
        };
    }

Using a for loop to iterate between elements during tests instead of using it.each:

    for (let user of users) {
      const name = screen.getByRole('cell', { name: user.name });
      const email = screen.getByRole('cell', { name: user.email });
      expect(name).toBeInTheDocument();
      expect(email).toBeInTheDocument();
    }

If you don't find byRole probably you don't have an ID assigned:

    // Test selector
    const inputEmail = screen.getByRole("textbox", { name: /email/i });

    // React Component
    <div>
        <label
          htmlFor="name" // if its not pointing to ID the getByRole will confuse the dom
          className="user-form__label">
          name
        </label>
        <input
          id="name" // NOTE: If you remove this line you never getByRole to work
          className="user-form__input"
          name="name"
          value={name}
          onChange={handleChange}
        />
      </div>

aria-label to select the getByRle with the name for buttons will work
Note that aria-label break the React convention:
Example:

    // React Component
    <button aria-label="sign-in">
    SignIn
    </button>

    // Test selector
    const signInButton = screen.getByRole('button', { name: /sign in/i})

Test an excepction way.

    //imagine textbox does not exist
    expect(
    () => screen.getByRole('textbox').toThrow()
    )

Query does not throw exception, use that to test if element does not exist

    //imagine textbox does not exist
    expect(screen.queryByRole('textbox').toEqual('null'))
    const element = screen.queryByRole(element)
    expect(element).not.ToBeInTheDocument()

findBy is completely async

```javascript
let errorThrown = false;
try {
  await screen.findByRole("textbox");
} catch (err) {
  errorThrown = true;
}
expect(errorThrown).toEqual(true);
```

custom Matches

```javascript
function toContainRoleCustom(container, role, qty = 1) {
  if (elements.length === quantity) {
    return {
      pass: true,
    };
  }
}
return {
  pass: false,
  message: () =>
    `Expected to find ${quantity} ${role} elements. Found ${elements.length} instead.`,
};
expect.extend({ toContainRole });
```

```javascript
test("the form displays two buttons", () => {
  render(<FormData />);

  const form = screen.getByRole("form");

  expect(form).toContainRole("link", 10);
});
```
