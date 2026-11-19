**pass**

- Each secret is a GPG-encrypted file in a folder, usually tracked with git.
- **Weak spot 1, metadata:** Entry names aren’t encrypted. Anyone who can see the store sees names like `uc-controller/ansible-vault`, though not the passwords.
- **Weak spot 2, GPG:** GPG is hard to use well. Most real-world failures are key handling mistakes, not broken encryption.
- **Age:** The current version is pass 1.7.4, a shell script, with GnuPG encrypting each password file. It changes rarely, which you could see as stable or as stagnant. [tech-insider](https://tech-insider.org/de/?p=1904)

**Alternatives that keep the same model:**

- **gopass:** Built as a drop-in replacement for pass and actively maintained, with 1.17.0 released on 30 August 2026. It adds recipient management to easily manage multiple users of each store. That matters once you have more than one operator, or clients who hold their own vault passwords. [Gopass: Open-source command-line password manager for teams +2](https://www.helpnetsecurity.com/2026/09/21/gopass-open-source-password-manager/)
- **passage:** A fork of password-store that uses age as its backend instead of GnuPG. The encryption is simpler and harder to get wrong, but the tool is younger and has fewer integrations. [github](https://github.com/FiloSottile/passage)

**Connecting any of them to Ansible:** All three work the same way. If you store your passwords in a third-party tool, you need a vault password client script to retrieve them, and the script must have a name ending in -client or -client.EXTENSION, print the password to standard output, and accept a --vault-id option. Your vault-client.py would become a short wrapper around `pass show` (or the gopass or passage equivalent). That also fixes tonight’s root problem: the vault password never needs to be copied or typed by hand, so it can’t end up in the wrong prompt. [ansible](https://docs.ansible.com/ansible/latest/vault_guide/vault_managing_passwords.html)[ansible](https://docs.ansible.com/projects/ansible-core/2.13/user_guide/vault.html)

**My take:**

- **For you alone right now:** pass is fine.
- **For the operator-facing design:** I’d lean to gopass. It reads the same store format, so you can start with pass and switch later without migrating.
- **Hardware key:** Either way, keep the GPG key on a hardware token if you have one.

**The operator document:** Once Claude Code reports how the vault password got into the batch, I can write it as web-ready Markdown, like the worktree guide. It would cover:

- where the vault password lives and how Ansible fetches it;
- how batch passwords are generated, stored, retired and set on a node;
- how to rekey;
- a rule that the two kinds of password never pass through the same prompt.

Should I start it now with the parts that are already decided, or wait for the investigation?