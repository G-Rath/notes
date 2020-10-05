# RubyMine (IntelliJ Ultimate with Ruby Plugin)

Syncing installed gems without having to recopy all gems (which is _slow_):

```
rsync -cr --delete /home/g-rath/.rbenv/versions/2.6.6/lib/ruby/gems/2.6.0/gems/<gem>-<version> /c/Users/G-Rath/AppData/Local/JetBrains/IntelliJIdea2020.2/ruby_stubs/-672385609/home/g-rath/.rbenv/versions/2.6.6/lib/ruby/gems/2.6.0/gems/
```

Example:

```
rsync -cr --delete /home/g-rath/.rbenv/versions/2.6.6/lib/ruby/gems/2.6.0/gems/bourbon-7.0.0 /c/Users/G-Rath/AppData/Local/JetBrains/IntelliJIdea2020.2/ruby_stubs/-672385609/home/g-rath/.rbenv/versions/2.6.6/lib/ruby/gems/2.6.0/gems/
```

This is the command that RubyMine uses to sync when using ruby via WSL as a
remote interpreter.
