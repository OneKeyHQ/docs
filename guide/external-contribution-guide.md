# Contributing

Choose the repository that owns the feature you want to change. Read its README, contribution instructions, and security policy before starting.

| Area                              | Repository                                                                 |
| --------------------------------- | -------------------------------------------------------------------------- |
| OneKey apps and browser extension | [app-monorepo](https://github.com/OneKeyHQ/app-monorepo)                   |
| Hardware SDK and developer portal | [hardware-js-sdk](https://github.com/OneKeyHQ/hardware-js-sdk)             |
| Injected wallet providers         | [cross-inpage-provider](https://github.com/OneKeyHQ/cross-inpage-provider) |
| Firmware                          | [firmware](https://github.com/OneKeyHQ/firmware)                           |
| This GitBook collection           | [docs](https://github.com/OneKeyHQ/docs)                                   |

## Propose a change

For a new chain or a change to an existing integration, describe the runtime, signing methods, serialization format, and required device or firmware support. A software-wallet integration and a hardware-signing integration may require changes in different repositories.

Check existing issues and pull requests before opening a new proposal. Keep the scope small enough to review and include reproduction steps or test evidence where relevant.

## Submit a fix

1. Follow the selected repository's development instructions.
2. Make the change and update the affected examples or documentation.
3. Run the checks required by that repository.
4. Open a pull request explaining the problem, the resulting behavior, and what you verified.

For documentation reports, include the page URL and the incorrect statement or example. For security vulnerabilities, follow the affected repository's security policy instead of publishing exploit details in an issue.
