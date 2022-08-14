# A Serious GitHub Account Setup Guide

In this guide, I'm going to show you how to configure your GitHub account to
improve security and privacy. This guide is somewhat opinionated about some
settings, feel free to adjust as you wish.

The GitHub documentation is very complete, so for specific instructions and
commands, follow the links and apply the recommendations in this guide.

## Enable 2FA

- Enable [two-factor authentication](https://docs.github.com/en/authentication/securing-your-account-with-two-factor-authentication-2fa/configuring-two-factor-authentication) - follow the instructions in the GitHub
docs.
- Associate your 2FA with a password manager with TOTP support.
    - Consider a password manager instead of a specialized TOTP auth
    app (see the info box below).
    - Don't use SMS.
- Store your recovery codes in a secure location.
    - I strongly recommend storing the recovery codes in a password manager as
    well. Even if you lose your phone, you can access the password manager via
    browser and retrieve the recovery codes.
- Optional: Consider configuring a [security key](https://docs.github.com/en/authentication/securing-your-account-with-two-factor-authentication-2fa/configuring-two-factor-authentication#configuring-two-factor-authentication-using-a-security-key).
A security key will allow you to log in using Face ID, Touch ID, etc., or even a
YubiKey.

!!! info "It's time for a password manager"
    If you don't use a password manager, it's time for you to consider one.
    Instead of relying on the browsers' password storage across multiple devices
    (which is ok but not great) consider creating an account in a password
    manager service and centralizing your passwords, TOTP access, and other
    sensitive notes.

    I use [1Password](https://1password.com/) and recommend it as a user
    (I have no commercial affiliation with them). It's not free but a personal
    account is really cheap. Another interesting alternative is [bitwarden](https://bitwarden.com/).
    I haven't used bitwarden, but I have heard good things about them.

## Configure SSH key

- [Generate an SSH key](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent) to access GitHub via
the git CLI.
- Make sure you add the SSH key to the ssh-agent.
It will make things transparent for you in the day-to-day.
- Don't use username and password to fetch and push code.

## Email Privacy Settings

- Go to the [email settings](https://github.com/settings/emails) and mark
the checkbox `Keep my email addresses private`.
- Set your no-reply email `<username>@users.noreply.github.com` as the [commit email address in Git](https://docs.github.com/en/account-and-profile/setting-up-and-managing-your-personal-account-on-github/managing-email-preferences/setting-your-commit-email-address#setting-your-commit-email-address-in-git).
```shell
git config --global user.email "<username>@users.noreply.github.com"
```
- In the same [email settings](https://github.com/settings/emails) page, mark
the checkbox `Block command line pushes that expose my email` -
[see here an explanation](https://docs.github.com/en/account-and-profile/setting-up-and-managing-your-personal-account-on-github/managing-email-preferences/blocking-command-line-pushes-that-expose-your-personal-email-address).
- [See this documentation for more details](https://docs.github.com/en/account-and-profile/setting-up-and-managing-your-personal-account-on-github/managing-email-preferences/setting-your-commit-email-address).

!!! warning "GitHub emails used in commits are publicly available to anyone"
    Every commit pushed to public GitHub repos is easily available via the
    GitHub API, and so are the emails used in these commits - [see an example
    here](https://api.github.com/repos/torvalds/linux/commits/f6eb0fed6a3957c0b93e3a00c1ffaad84d4ffc31).

## Configure Commit Signature Verification

- See the [commit signature verification](https://docs.github.com/en/authentication/managing-commit-signature-verification/about-commit-signature-verification) page for an explanation.
- [Generate a **new** GPG key](https://docs.github.com/en/authentication/managing-commit-signature-verification/generating-a-new-gpg-key)
for GitHub.

    !!! warning
        Avoid using an existing key since GitHub will leak all emails assigned to
        your GPG key regardless if they are associated with your GitHub account or
        not - [see this example](https://api.github.com/users/dschuermann/gpg_keys),
        you can find 6 distinct emails at the moment of this writing, including
        emails that have not to do with GitHub.

    - When prompted for the GPG key creation, use the following recommendations:
        - `Please select what kind of key you want`: Use a sing only key
        `(4) RSA (sign only)`.
        - `What keysize do you want?`: Use the size recommended in the GitHub
        docs `4096`.
        - `Key is valid for?`: Specify a time to live `5y`.
        - `Email address`: Use your no-reply address `<username>@users.noreply.github.com`.
        - And, of course, set a reosanable passphrase.

- Follow the remaining steps to [add your GPG key to GitHub](https://docs.github.com/en/authentication/managing-commit-signature-verification/adding-a-gpg-key-to-your-github-account)
and [configure Git with your signing key](https://docs.github.com/en/authentication/managing-commit-signature-verification/telling-git-about-your-signing-key).
- Enable [vigilante mode](https://docs.github.com/en/authentication/managing-commit-signature-verification/displaying-verification-statuses-for-all-of-your-commits). After you save your
GPG public key on GitHub, on the [same page](https://github.com/settings/keys)
mark the checkbox `Flag unsigned commits as unverified`. If anyone tries to
impersonate you, the commit will be flagged as `Unverified`.

!!! info "Why are we configuring commit signature verification?"
    Git impersonation is a real problem that, while most of the time is used
    only for bad jokes, can cause a lot of confusion and bring some real
    software supply chain concerns.

    There is plenty of content on the Internet about this subject. A good one
    is the blog post [How to Spoof Any User on Github…and What to Do to Prevent It](https://blog.gruntwork.io/how-to-spoof-any-user-on-github-and-what-to-do-to-prevent-it-e237e95b8deb).
