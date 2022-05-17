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

.. code-block:: text

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

1Password for login with n

Translated with www.DeepL.com/Translator (free version)
