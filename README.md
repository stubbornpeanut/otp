# otp: One Time Password utilities Go / Golang

[![PkgGoDev](https://pkg.go.dev/badge/github.com/stubbornpeanut/otp)](https://pkg.go.dev/github.com/stubbornpeanut/otp)

## Why Fork pquerna/otp?

Guardtower leverages OTP to support multi-factor authentication.  Because this product is
security-sensitive, and with all the recent supply-chain attacks, I thought it best to
fork [pquerna/otp](https://github.com/stubbornpeanut/otp), review the code, and then be able to
review and filter any updates that looked suspicious.  

Furthermore, the code hasn't been tagged with a new release in 18 months, which worried me
a bit.  This allows me to tag releases at my own pace and keep the software up to date as
needed.

Note there are no additional features in this version of pquerna/otp.  It is purely for my
personal use, though you are welcome to use it in your own applications, obviously.

## Why One Time Passwords?

One Time Passwords (OTPs) are an mechanism to  improve security over passwords alone. When
a Time-based OTP (TOTP) is stored on a user's phone, and combined with something the user
knows (Password), you have an easy on-ramp to [Multi-factor
authentication](http://en.wikipedia.org/wiki/Multi-factor_authentication) without adding a
dependency on a SMS provider.  This Password and TOTP combination is used by many popular
websites including Google, GitHub, Facebook, Salesforce and many others.

The `otp` library enables you to easily add TOTPs to your own application, increasing your
user's security against mass-password breaches and malware.

Because TOTP is standardized and widely deployed, there are many [mobile clients and
software
implementations](http://en.wikipedia.org/wiki/Time-based_One-time_Password_Algorithm#Client_implementations).

## Features

* Generating QR Code images for easy user enrollment.
* Time-based One-time Password Algorithm (TOTP) (RFC 6238): Time based OTP, the most
  commonly used method.
* HMAC-based One-time Password Algorithm (HOTP) (RFC 4226): Counter based OTP, which TOTP
  is based upon.
* Generation and Validation of codes for either algorithm.

## Implementing TOTP in Your Application

### User Enrollment

For an example of a working enrollment work flow, [GitHub has documented theirs](https://help.github.com/articles/configuring-two-factor-authentication-via-a-totp-mobile-app/
),  but the basics are:

1. Generate new TOTP Key for a User. `key,_ := totp.Generate(...)`.
1. Display the Key's Secret and QR-Code for the User. `key.Secret()` and `key.Image(...)`.
1. Test that the user can successfully use their TOTP. `totp.Validate(...)`.
1. Store TOTP Secret for the User in your backend. `key.Secret()`
1. Provide the user with "recovery codes". (See Recovery Codes bellow)

### Code Generation

* In either TOTP or HOTP cases, use the `GenerateCode` function and a counter or
  `time.Time` struct to generate a valid code compatible with most implementations.
* For uncommon or custom settings, or to catch unlikely errors, use `GenerateCodeCustom`
  in either module.

### Validation

1. Prompt and validate User's password as normal.
1. If the user has TOTP enabled, prompt for TOTP passcode.
1. Retrieve the User's TOTP Secret from your backend.
1. Validate the user's passcode. `totp.Validate(...)`

### Recovery Codes

When a user loses access to their TOTP device, they would no longer have access to their
account.  Because TOTPs are often configured on mobile devices that can be lost, stolen or
damaged, this is a common problem. For this reason many providers give their users "backup
codes" or "recovery codes".  These are a set of one time use codes that can be used
instead of the TOTP.  These can simply be randomly generated strings that you store in
your backend.  [Github's documentation provides an overview of the user experience](
https://help.github.com/articles/downloading-your-two-factor-authentication-recovery-codes/).

## License

`otp` is licensed under the [Apache License, Version 2.0](./LICENSE)
