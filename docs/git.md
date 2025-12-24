# Konfigurasi Git dan SSH

Saya menggunakan Macbook dan iterm. Di laptop saya ada 3 akun git

- github <adi.wahyu.p@gmail.com>
- github <adi.wahyu.p@labkreatif.com>
- bitbucket <adi.wahyu.p@univpancasila.ac.id>

ini isinya

```ssh
Host github.com
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_ed25519
  IdentitiesOnly yes

Host bitbucket.org
  HostName bitbucket.org
  User git
  IdentityFile ~/.ssh/id_bitbucket_univ
  IdentitiesOnly yes

# --- Akun Kantor (Lab Kreatif) ---
Host github-labkreatif
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_ed25519_labkreatif
  IdentitiesOnly yes
```

---

## Project Repository

**FSRD Project Repository:**

- **URL:** <https://github.com/awplk/fsrd>
- **Branch:** `main`
- **Remote:** `origin`

**Struktur:**

- `docs/` - Dokumentasi
- `ikafsrds-digital-home/` - React project (embedded repo)
- `ikafsrds-static/` - Static HTML/CSS/JS version (embedded repo)

**Push terakhir:** 24 Desember 2024
