Security Framework
==================

There are many articles on how to solve one particular problem,
but there is no ready-made comprehensive solution to cover most problems.
This is why this project is called a framework, no choice is intended,
only specific solutions. This framework can be used as a step by step
instructions.

Do I need it? All people have inertia of thought, it's hard to change established
habits, but at some point there will be something to lose, and the methods of protection will be at the same
low level. Also, many people tend to underestimate their importance and
the size of the vulnerability they create. For example, by compromising yourself
you compromise your employer as well. By contributing to Open Source you
you endanger all the people who use your code.

.. contents::
  Content:

We'll need
~~~~~~~~~~~~~~~

- `Google Authenticator`_ - free phone authenticator
- 1Password is a password manager, $2.99/month
- `YubiKey 5-series`_ - hardware security key, requires two of these,
  from $45 each

What is YubiKey?

.. figure:: assets/keys.jpg

  Source https://itsfoss.com/yubikey-5c-nfc/

This is a hardware security key from Yubico. It is primarily
It is primarily designed for two-factor authentication which is extremely
tolerant to phishing. Just to give you an idea of how cool this device is.
I will tell you that we will generate a private SSH key right inside the YubiKey, this
private key will be stored there all the time and there is no way to read
or copy it from there physically, and in order to use this SSH key
you need the hardware key itself and knowledge of the PIN code, which is protected from
the PIN code which is protected by the key itself.

YubiKey is not the only variant of such keys, there is also Titan from Google,
Thesis and several other manufacturers. All of them implement open standards,
so they are interchangeable. The difference is in what functionality they provide
and what standards they support. For two-factor authentication, the relevant
For two-factor authentication the relevant standard is FIDO2. YubiKey additionally supports
PIV and OpenPGP. We are interested in the standard PIV, work with it will be described below.
It's also worth noting that YubiKey has no inside batteries and moving parts, their
The design is the most reliable and durable.

Yubico company has many variants of keys, you can get confused.
Relevant are the keys of Series 5, we can say that they can do everything. But, if
you only need a key for two-factor authentication, you can save
and buy `YubiKey Security Key` (with FIDO2 standard support).

You need two hardware security keys, as in the case of machine keys
You must always have a second spare key. It is possible to lose a security key together with the phone.
with the phone and you will definitely need a second extra security key.

Example. I have a laptop with only USB-C ports and have an adapter for USB-A, and
A desktop computer with all the ports possible. I chose myself
`YubiKey 5C NFC`_ as the primary and `YubiKey 5 NFC`_ as a spare key.

The big picture
~~~~~~~~~~~~~

It's useful to visualize such a hierarchy, which shows what
security of each of the services is based on. The simpler this configuration is, the
the better.


  YubiKey && Authenticator
  ├─ 1Password
  │ ├── Apple
  │├─ Google
  │ ├─ Bank
  │ └─ ...
  └─ SSH
     └─ Git

In this configuration our whole security is based on the hardware keys
YubiKey and the authenticator application. And also on us: we are responsible for
for keeping the passwords in our memory responsibly. If something is wrong at this level.
something is wrong, everything else will be in doubt.

Passwords
~~~~~~

There is not one simple rule for making passwords. The rules vary
depending on the task at hand. The basic criteria are:

- whether or not you have to remember the password
- whether or not there's a protection against brute force password guessing.

The most important: passwords should not be repeated and there should be no possibility
them knowing something about you, like your date of birth, your
your relatives and friends, your hobbies.

Master Password Manager password
---------------------------------

Passwords made up of letters, digits and symbols are usually considered the strongest passwords.
In fact, these requirements often lead to bad passwords, people are bad at
at coming up with random passwords because they are hard to remember.

The 1Password website has useful information on this topic:

- https://blog.1password.com/how-long-should-my-passwords-be/
- https://support.1password.com/strong-master-password/

In short, choose a multi-word value for your Master Password, such as:

.. code-block:: text

  glazing-quetzal-big-bold-pullback

This password was generated here:
https://1password.com/password-generator/?type=memorable.
It's easy to remember, easy to pronounce, and easy to type blindly on the
keyboard. And most importantly, it is many orders of magnitude harder to guess than your
current password of letters, numbers and special characters.

Secret key
++++++++++++++

The secret key
++++++++++++++

1Password to log in from a new device requires not only a Master password, but also
secret key: https://support.1password.com/secret-key-security/. This
an additional degree of protection dramatically increases the protection of your data, but also
creates some inconvenience. If the Master password can be remembered, then the secret
a 34 character key needs to be stored somewhere. Disable this feature
you can't, even if you're using two-factor authentication.

The secret key is generated during registration in 1Password, you will be prompted
download 1Password Emergency Kit as a PDF file, which will contain the secret itself
key and QR code for quick app setup. 1Password offers to store
it in printed form or on a special flash drive somewhere in the safe. In this and
is an inconvenience.

To be fair, it should be clarified that in the case when you lose your phone, but
you still have a laptop with you, then you can read the secret key with
laptop. The main thing is that one of the authenticated
devices.

But since we will use YubiKey keys, we can use them like
data store. How to use this will be described below:
`Secret key to the password manager`_.

Password for phone and computer
--------------------------------

Modern Apple phones and computers are equipped with hardware storage
personal information (Secure Enclave), this vault will not allow
try your password as many times as you want. But that's all for making a good password
should still be taken very seriously.

For example, few people realize that Apple allows you to perform certain actions
associated with your Apple ID without asking for your Apple ID password,
it will be enough to enter the password from the phone / computer. So bad
your phone/computer password will negate a good Apple ID password.

Here it is probably worth repeating again: passwords should not be repeated and should not
be able to pick them up knowing something about you, such as your date of birth,
your relatives and friends, your hobbies.

- for the password from the device, you can use the same method as for
  Master password to password manager
- iPhone allows you to set a password of any complexity, not just a 6-digit code
- it is better to increase the complexity of the password due to its length, and not due to
  using special characters:

  * on a computer, the hands remember where the letters are located on the keyboard better than
    where special characters are located. When touch dialing, entering a letter
    password will be faster and with fewer errors, this will enable
    increase password length
  * on the phone, entering special characters requires pressing two buttons - buttons
    ``123`` and special character buttons, this slows down password entry a lot

2FA or MFA
~~~~~~~~~~~

Types of the second factor, sorted by the level of protection:

- phone (low security level)
- push notifications
- authenticator
- hardware security key (high level of protection)
- biometric hardware security key

Different services provide different levels of support for two-factor
authentication. Somewhere you can use only a phone number, somewhere
all possible methods can be used. The ideal option will be described below.
service that supports both authenticators and hardware security keys
(Google, 1Password, GitHub). For other services:

- choose the best level of protection (see the list above)
- when it is not possible to configure more than one type of the second factor
  authentication, **be sure to save the recovery codes** of access in
  password manager

Telephone number
----------------

Most likely you have already set up your phone number as a second factor
authentication on some services. After setting up the authenticator app
and two hardware YubiKey keys, phone number as a second factor
authentication should be disabled. Otherwise, it will become the weakest link that
will negate the benefit of hardware security keys.

Authenticator
--------------

Authenticator application is worse in terms of security than hardware ones
security keys, it does not prevent phishing attacks, so use
they are not worth it in daily practice. We need an authenticator as another
kind of second factor for more redundancy, and many services will require you to
first register authenticator as second
