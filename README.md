# Mastering the Art of Testing with React Testing Library

https://testing-library.com/docs/react-testing-library/intro/

### Overview of newly acquired knowledge

Tool that shows a web to facilitate to locate elements.
Remember to add a border if you cant select the element.

    screen.logTestingPlaygroundURL()

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

### Command tool

    With the jest watcher running you can filter the test pressing p (on main menu, after "w" for show More)
    Or O to run tests for changed files

### ACT: (a window where just exist after all the async is executd)

ACT is a window that only appears once all asynchronous operations have been completed.
The following RTL functions automatically call ACT for you, ensuring that the element or condition is present before proceeding:

    screen.findByRole
    screen.findAllByRole
    waitFor
    user.keyboard
    user.click

Highlight: Use FindBy, does not follow the Warning throw by the test!
