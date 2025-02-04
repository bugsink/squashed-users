# Squash migrations in django.contrib.auth for better performance and nicer output

As requested on the [Django Ticket Tracker](https://code.djangoproject.com/ticket/35707), but as a separate package.

### Why?

* Faster migrations (for every (test) run)!
* Less pollution on screen

Counter-arguments about supposed breaking changes, stability etc. can be simply refuted by the fact that all such
arguments would equally apply to _any_ use of squashed migrations.

I believe in `squashmigrations`, so let's just "squash all the things"!

### Usage

```
pip install squashed-users
# migrate as usual
```

### How it's made

On Django 4.2, the following was run:

```
pyton manage.py squashmigrations auth 0001 0012
```

The comments in the generated file were followed (copying RunMigration code over).

Then, the file was moved from my virtualenv to this package.

(No django.contrib.auth migrations have been made between Django 4.2 and 5.2, so this works for any supported Django
version)

### A Magic Wheel A.K.A. package-level Monkey-patching

`django/contrib/auth/migrations/0001_squashed_0012_alter_user_first_name_max_length.py` is the only file in this
"package", no `__init__.py` in any of the directories, and nothing else.

Surprisingly, such a setup "works" in the sense that this squashed migration ends up in the right location, whether you
install this package or Django first. Django then sees the file, and the usual migration magic kicks in from there.
