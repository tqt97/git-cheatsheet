# Git Engineering Lab

> **A practical Git reference for developers.**
> Learn Git by purpose, not by memorizing commands.

![License](https://img.shields.io/badge/license-MIT-green)
![Status](https://img.shields.io/badge/status-MVP-blue)
![Git](https://img.shields.io/badge/git-reference-orange)

---

## Overview

Git Engineering Lab là một bộ tài liệu Git được xây dựng theo hướng **Engineering Handbook**.

Mục tiêu của dự án không phải dạy Git từ đầu, mà cung cấp một nơi để:

- Tra cứu command nhanh
- Hiểu bản chất hoạt động của Git
- Chọn đúng command cho từng tình huống
- Áp dụng Best Practices trong dự án thực tế

Repository được thiết kế để có thể sử dụng hằng ngày như một "Git Reference".

---

## Who is this for?

- Junior Developer
- Middle Developer
- Senior Developer
- Tech Lead

---

## Learning Philosophy

Thay vì học theo command:

```
git add

git commit

git push

git pull
```

Repository được tổ chức theo **mục đích sử dụng**.

Ví dụ:

> Muốn bỏ commit cuối

↓

```
Undo & Recovery
```

Thay vì phải nhớ command là:

```
git reset

git revert

hay

git restore
```

---

## Repository Structure

```
git-engineering-lab/

README.md

cheatsheet/
handbook/
playbook/
```

---

## Documentation Structure

### 1. Cheat Sheet

Tra cứu nhanh command theo workflow.

Ví dụ:

```
Stage & Commit

↓

git add

git commit

git commit --amend
```

Không giải thích internals.

Mục tiêu:

> Tìm command trong vài giây.

---

### 2. Handbook

Giải thích WHY.

Ví dụ:

- Working Tree
- Index
- Commit
- Branch
- HEAD
- Remote
- History Rewrite

---

### 3. Playbook

Tình huống thực tế.

Ví dụ:

- Detached HEAD
- Merge Conflict
- Lost Commit
- Wrong Branch
- Force Push

---

## Cheat Sheet

### Repository

| File | Description |
|------|-------------|
| [Setup & Config](./cheatsheet/01-setup-and-config.md) | Install, configure Git |
| [Create & Clone](./cheatsheet/02-create-and-clone.md) | Initialize and clone repositories |
| [Inspect Status & History](./cheatsheet/03-inspect-status-and-history.md) | View repository status and history |
| [Stage & Commit](./cheatsheet/04-stage-and-commit.md) | Stage and create commits |
| [Branch & Switch](./cheatsheet/05-branch-and-switch.md) | Branch management |
| [Sync With Remote](./cheatsheet/06-sync-with-remote.md) | Fetch, pull, push |
| [Merge & Rebase](./cheatsheet/07-merge-and-rebase.md) | Merge history |
| [Undo & Recovery](./cheatsheet/08-undo-and-recovery.md) | Undo and recover changes |
| [Stash & Cherry-pick](./cheatsheet/09-stash-and-cherry-pick.md) | Temporary work |
| [Tags & Release](./cheatsheet/10-tags-and-release.md) | Version tagging |
| [Decision Matrix](./cheatsheet/11-decision-matrix.md) | Which command should I use? |
| [Best Practices](./cheatsheet/12-best-practices.md) | Engineering recommendations |

---

## Handbook

Coming soon.

- Git Mental Model
- Working Tree
- Index
- Commit
- Branch
- HEAD
- Remote
- History Rewrite

---

## Quick Start

### Clone

```bash
git clone https://github.com/<your-org>/git-engineering-lab.git
```

### Open Cheat Sheet

```
cheatsheet/
```

Chọn đúng workflow bạn đang cần.

Ví dụ:

```
Undo & Recovery
```

hoặc

```
Merge & Rebase
```

---

## Recommended Learning Order

### New to Git

```
Setup

↓

Create Repository

↓

Stage & Commit

↓

Branch

↓

Remote

↓

Merge

↓

Recovery
```

---

### Daily Development

```
Stage & Commit

↓

Branch

↓

Remote

↓

Decision Matrix

↓

Best Practices
```

---

### Troubleshooting

```
Undo & Recovery

↓

Playbook
```

---

## Contributing

Contributions are welcome.

Please read:

```
CONTRIBUTING.md
```

before creating a Pull Request.

---

## Roadmap

### MVP

- [ ] Cheat Sheet
- [ ] Handbook
- [ ] Playbook

### Future

- [ ] Labs
- [ ] Incident Library
- [ ] Enterprise Playbooks
- [ ] Git Internals

---

## License

MIT License.

---

## Support

If this project helps you:

- ⭐ Star the repository
- 🍴 Fork the repository
- 🐛 Open an Issue
- 🚀 Submit a Pull Request