### brew 통해 실행
```{.bash}
brew list
brew info appname
brew services start tomcat@7
```
### 경로 찾아서 실행
```{.bash}
brew info appname
/opt/homebrew/opt/tomcat@7/bin/catalina run
```

### 톰캣 경로 이동 후
```{.bash}
./catalina start
./catalina stop
```