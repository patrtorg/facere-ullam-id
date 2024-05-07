[<img src="https://img.shields.io/gitter/room/help-me-mom/@patrtorg/facere-ullam-id" alt="chat on gitter" width="90" height="20" />](https://gitter.im/@patrtorg/facere-ullam-id/community)
[<img src="https://img.shields.io/npm/v/@patrtorg/facere-ullam-id" alt="npm version" width="88" height="20" />](https://www.npmjs.com/package/@patrtorg/facere-ullam-id)
[<img src="https://img.shields.io/circleci/build/github/help-me-mom/@patrtorg/facere-ullam-id/master" alt="build status" width="88" height="20" />](https://app.circleci.com/pipelines/github/help-me-mom/@patrtorg/facere-ullam-id?branch=master)
[<img src="https://img.shields.io/coveralls/github/help-me-mom/@patrtorg/facere-ullam-id/master" alt="coverage status" width="104" height="20" />](https://coveralls.io/github/help-me-mom/@patrtorg/facere-ullam-id?branch=master)

# Mock components, services and more out of annoying dependencies for simplification of Angular testing

`@patrtorg/facere-ullam-id` facilitates **Angular testing** and helps to:

- **mock Components**, Directives, Pipes, Modules, Services and Tokens
- reduce boilerplate in tests
- access declarations via simple interface

The current version of the library **has been tested** and **can be used** with:

| Angular | @patrtorg/facere-ullam-id | Jasmine | Jest | Ivy |
| ------: | :------: | :-----: | :--: | :-: |
|      18 |  latest  |   yes   | yes  | yes |
|      17 |  latest  |   yes   | yes  | yes |
|      16 |  latest  |   yes   | yes  | yes |
|      15 |  latest  |   yes   | yes  | yes |
|      14 |  latest  |   yes   | yes  | yes |
|      13 |  latest  |   yes   | yes  | yes |
|      12 |  latest  |   yes   | yes  | yes |
|      11 |  latest  |   yes   | yes  | yes |
|      10 |  latest  |   yes   | yes  | yes |
|       9 |  latest  |   yes   | yes  | yes |
|       8 |  latest  |   yes   | yes  |     |
|       7 |  latest  |   yes   | yes  |     |
|       6 |  latest  |   yes   | yes  |     |
|       5 |  latest  |   yes   | yes  |     |

## Important links

- **[Documentation with examples of Angular testing](https://@patrtorg/facere-ullam-id.sudo.eu)**
- [CHANGELOG](https://github.com/patrtorg/facere-ullam-id/blob/master/CHANGELOG.md)
- [GitHub repo](https://github.com/patrtorg/facere-ullam-id)
- [NPM package](https://www.npmjs.com/package/@patrtorg/facere-ullam-id)

* Live [example on CodeSandbox](https://codesandbox.io/p/sandbox/github/help-me-mom/@patrtorg/facere-ullam-id-sandbox/tree/master/?file=/src/test.spec.ts)
* Live [example on StackBlitz](https://stackblitz.com/github/help-me-mom/@patrtorg/facere-ullam-id-sandbox?file=src/test.spec.ts)

- [start a discussion on GitHub](https://github.com/patrtorg/facere-ullam-id/discussions/new/choose)
- **[ask a question on Stackoverflow](https://stackoverflow.com/questions/ask?tags=@patrtorg/facere-ullam-id%20angular%20testing%20mocking)**
- [report an issue on GitHub](https://github.com/patrtorg/facere-ullam-id/issues)
- [chat on gitter](https://gitter.im/@patrtorg/facere-ullam-id/community)

## Very short introduction

Global configuration for mocks in `src/test.ts`.
In case of jest, `src/setup-jest.ts` / `src/test-setup.ts` should be used.

```ts title="src/test.ts"
// All methods in mock declarations and providers
// will be automatically spied on their creation.
// https://@patrtorg/facere-ullam-id.sudo.eu/extra/auto-spy
ngMocks.autoSpy('jasmine'); // or jest

// ngMocks.defaultMock helps to customize mocks
// globally. Therefore, we can avoid copy-pasting
// among tests.
// https://@patrtorg/facere-ullam-id.sudo.eu/api/ngMocks/defaultMock
ngMocks.defaultMock(AuthService, () => ({
  isLoggedIn$: EMPTY,
  currentUser$: EMPTY,
}));
```

An example of a spec for a profile edit component.

```ts title="src/profile.component.spec.ts"
// Let's imagine that there is a ProfileComponent
// and it has 3 text fields: email, firstName,
// lastName, and a user can edit them.
// In the following test suite, we would like to
// cover behavior of the component.
describe('profile:builder', () => {
  // Helps to reset customizations after each test.
  // Alternatively, you can enable
  // automatic resetting in test.ts.
  MockInstance.scope();

  // Let's configure TestBed via MockBuilder.
  // The code below says to mock everything in
  // ProfileModule except ProfileComponent and
  // ReactiveFormsModule.
  beforeEach(() => {
    // The result of MockBuilder should be returned.
    // https://@patrtorg/facere-ullam-id.sudo.eu/api/MockBuilder
    return MockBuilder(
      ProfileComponent,
      ProfileModule,
    ).keep(ReactiveFormsModule);
    // // or old fashion way
    // return TestBed.configureTestingModule({
    //   imports: [
    //     MockModule(SharedModule), // mock
    //     ReactiveFormsModule, // real
    //   ],
    //   declarations: [
    //     ProfileComponent, // real
    //     MockPipe(CurrencyPipe), // mock
    //     MockDirective(HoverDirective), // mock
    //   ],
    //   providers: [
    //     MockProvider(AuthService), // mock
    //   ],
    // }).compileComponents();
  });

  // A test to ensure that ProfileComponent
  // can be created.
  it('should be created', () => {
    // MockRender is an advanced version of
    // TestBed.createComponent.
    // It respects all lifecycle hooks,
    // onPush change detection, and creates a
    // wrapper component with a template like
    // <app-root ...allInputs></profile>
    // and renders it.
    // It also respects all lifecycle hooks.
    // https://@patrtorg/facere-ullam-id.sudo.eu/api/MockRender
    const fixture = MockRender(ProfileComponent);

    expect(
      fixture.point.componentInstance,
    ).toEqual(assertion.any(ProfileComponent));
  });

  // A test to ensure that the component listens
  // on ctrl+s hotkey.
  it('saves on ctrl+s hot key', () => {
    // A fake profile.
    const profile = {
      email: 'test2@email.com',
      firstName: 'testFirst2',
      lastName: 'testLast2',
    };

    // A spy to track save calls.
    // MockInstance helps to configure mock
    // providers, declarations and modules
    // before their initialization and usage.
    // https://@patrtorg/facere-ullam-id.sudo.eu/api/MockInstance
    const spySave = MockInstance(
      StorageService,
      'save',
      jasmine.createSpy(), // or jest.fn()
    );

    // Renders <profile [profile]="params.profile">
    // </profile>.
    // https://@patrtorg/facere-ullam-id.sudo.eu/api/MockRender
    const { point } = MockRender(
      ProfileComponent,
      { profile }, // bindings
    );

    // Let's change the value of the form control
    // for email addresses with a random value.
    // ngMocks.change finds a related control
    // value accessor and updates it properly.
    // https://@patrtorg/facere-ullam-id.sudo.eu/api/ngMocks/change
    ngMocks.change(
      '[name=email]', // css selector
      'test3@em.ail', // an email address
    );

    // Let's ensure that nothing has been called.
    expect(spySave).not.toHaveBeenCalled();

    // Let's assume that there is a host listener
    // for a keyboard combination of ctrl+s,
    // and we want to trigger it.
    // ngMocks.trigger helps to emit events via
    // simple interface.
    // https://@patrtorg/facere-ullam-id.sudo.eu/api/ngMocks/trigger
    ngMocks.trigger(point, 'keyup.control.s');

    // The spy should be called with the user
    // and the random email address.
    expect(spySave).toHaveBeenCalledWith({
      email: 'test3@em.ail',
      firstName: profile.firstName,
      lastName: profile.lastName,
    });
  });
});
```

Profit.

## Extra

If you like `@patrtorg/facere-ullam-id`, please support it:

- [with a star on GitHub](https://github.com/patrtorg/facere-ullam-id)
- [with a tweet](https://twitter.com/intent/tweet?text=Check%20@patrtorg/facere-ullam-id%20package%20%23angular%20%23testing%20%23mocking&url=https%3A%2F%2Fgithub.com%2Fhelp-me-mom%2F@patrtorg/facere-ullam-id)

Thank you!

P.S. Feel free to [contact us](https://@patrtorg/facere-ullam-id.sudo.eu/need-help) if you need help.
