# ngx-otp-input

![GitHub License](https://img.shields.io/github/license/pkovzz/ngx-otp-input)

:warning: **Important note:** starting with version 1.0.0, the library has been completely rewritten to use standalone components and introduce breaking changes. As the section "Requirements" states, the library now requires Angular 14 or above.

TODO: copy the final version of the README.md to library's root folder

## What is this?

This is a simple Angular library that allows you to create an OTP (One Time Password) form by providing a set of options. The library is designed to be easy to use and highly customizable, allowing you to configure the form to suit your needs. If you like the library, please consider giving it a star on GitHub.

TODO: Vercel and Stackblitz demo links

## Requirements

To use this library, your project must be running **Angular 14** or above. This requirement stems from our adoption of standalone components, which are an integral feature in Angular's modern development approach. Standalone components offer a more streamlined and modular way to manage your components and simplify the dependency management, positioning them as the future of Angular development.

## Installation

To install this library, run:

```bash
npm install ngx-otp-input --save
```

## Example Usage

Since the library uses standalone components, you can directly import and use them in your Angular application without needing to declare them in any module. For more configuration options, refer to the [Configuration options](#configuration-options) section.

```typescript
import { Component } from '@angular/core';
import { NgxOtpInputComponent, NgxOtpInputComponentOptions } from 'ngx-otp-input';

@Component({
  selector: 'app-root',
  standalone: true,
  imports: [NgxOtpInputComponent],
  template: `
    <h1>Welcome to My Angular App</h1>
    <ngx-otp-input [options]="otpOptions"></ngx-otp-input>
  `,
  styleUrls: ['./app.component.scss'],
})
export class AppComponent {
  otpOptions: NgxOtpInputComponentOptions = {...};
}
```

## Inputs

### `options: NgxOtpInputComponentOptions`

The `options` input is an object that allows you to configure the OTP form. For a list of available options, refer to the [Configuration options](#configuration-options) section.

### `status: NgxOtpStatus | null | undefined`

The `status` input is a string that allows you to set the status of the OTP form. The status can be one of the following values: `null`, `undefined`, `'success'` or `'failed'`. This status is only used to visually indicate the result of the OTP verification process.

For type safety, you can use the `NgxOtpStatus` enum:

```typescript
import { NgxOtpStatus } from 'ngx-otp-input';

@Component({
  selector: 'app-root',
  standalone: true,
  imports: [NgxOtpInputComponent],
  template: ` <ngx-otp-input [status]="otpStatusEnum.SUCCESS"></ngx-otp-input> `,
})
export class AppComponent {
  status = NgxOtpStatus;
}
```

### `disabled: boolean`

The `disabled` input is a boolean that allows you to disable the OTP form. When set to `true`, the form will be disabled and the user will not be able to interact with it.

## Outputs

### `otpChange: string[]`

The `otpChange` output is an event that is emitted whenever the OTP value changes. The event payload is an array of strings, where each string represents a value in the OTP form.

### `otpComplete: string`

The `otpComplete` output is an event that is emitted whenever the OTP form is completed. The event payload is string, which represents the complete OTP value.

## Configuration options

The `NgxOtpInputComponentOptions` interface allows you to configure the OTP form. The following options are available:

| Option               | Type     | Default value | Description                                                                                                                                                             |
| -------------------- | -------- | ------------: | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `otpLength`          | number   |             6 | The number of inputs in the OTP form, this field is `required`                                                                                                          |
| `autoFocus`          | boolean  |          true | Whether the first input should be focused sautomatically                                                                                                                |
| `autoBlur`           | boolean  |          true | Whether the form should be blurred on complete                                                                                                                          |
| `hideInputValues`    | boolean  |         false | Whether the input values should be shown as password fields                                                                                                             |
| `regexp`             | RegExp   |    /^[0-9]+$/ | The regular expression that the input values should match                                                                                                               |
| `showBlinkingCursor` | boolean  |          true | Whether the input fields should have a blinking cursor                                                                                                                  |
| `ariaLabels`         | string[] |            [] | An array of strings that represent the aria-labels for each input field. For more information, please refer to the [More on aria-labels](#more-on-aria-labels) section. |

## Styling

The library provides a set of CSS classes that you can use to style the OTP form. The following classes are available:

| Class name               | Description                            |
| ------------------------ | -------------------------------------- |
| `ngx-otp-input-form`     | The main container of the OTP form     |
| `ngx-otp-input-box`      | The input field of the OTP form        |
| `ngx-blinking-cursor`    | The blinking cursor of the input field |
| `ngx-otp-input-disabled` | The disabled state of the form         |
| `ngx-otp-input-filled`   | The filled state of an input field     |
| `ngx-otp-input-success`  | The success state of the form          |
| `nngx-otp-input-failed`  | The failed state of the form           |

### How to use the classes

Styling is quite simple, but you have to use the classes directly in **root** style file, otherwise it will not work:

```scss
ngx-otp-input {
  .ngx-otp-input-form {
    ...
  }
  .ngx-otp-input-box {
    ...
  }
  ...
}
```

## Reset the form

In order to reset the form, you can use the `reset` method of the `NgxOtpInputComponent`:

First, get a reference to the component in your template:

```html
<ngx-otp-input
  #otpInput
  [options]="otpOptions"
></ngx-otp-input>
```

Then, get a reference to the component in your component class:

```typescript
import { Component, ViewChild } from '@angular/core';
import { NgxOtpInputComponent } from 'ngx-otp-input';

@Component({
  selector: 'app-root',
  standalone: true,
  imports: [NgxOtpInputComponent],
  template: `
    <ngx-otp-input
      #otpInput
      [options]="otpOptions"
    ></ngx-otp-input>
  `,
})
export class AppComponent {
  @ViewChild('otpInput') otpInput: NgxOtpInputComponent;

  resetForm() {
    this.otpInput.reset();
  }
}
```

Under the hood, the `reset` method will clear all the input values and reset the form to its initial state. For more information, refer to the [Angular FormArray reset](https://angular.dev/api/forms/FormArray#reset) documentation.

## More on aria-labels

The `ariaLabels` option allows you to provide a set of strings that represent the aria-labels for each input field. This option is useful for making the form more accessible to users who rely on screen readers. The `aria-label` attribute provides a way to specify a string that labels the current element, which can be read by screen readers to provide additional context to the user. The library will automatically assign the `aria-label` attribute to each input with a default value of `One Time Password Input Number` followed by the input index. However, you can override this default value by providing your own set of labels in the `ariaLabels` option.

If you provide an array of strings in the `ariaLabels` option, the library will use the values in the array to assign the `aria-label` attribute to each input field. The array should contain the same number of strings as the `otpLength` option, with each string representing the label for the corresponding input field. If the array contains fewer strings than the `otpLength` option, the library will use the default value for the remaining input fields.

## Side notes

If `hideInputValues` is set to `true`, the input values will be hidden by default, using the `password` input type. However certain password managers may place their browser extension icon on the input field, which may interfere with the input field's appearance.

## Contributing

If you would like to contribute to this project, please refer to the [CONTRIBUTING](docs/CONTRIBUTING.md) file for more information.

## Code of Conduct

Please read the [CODE OF CONDUCT](docs/CODE_OF_CONDUCT.md) file for more information.

## Changelog

See the [CHANGELOG](docs/CHANGELOG.md) file for details.

## License

This project is licensed under the MIT License - see the [LICENSE](docs/LICENSE) file for details.
