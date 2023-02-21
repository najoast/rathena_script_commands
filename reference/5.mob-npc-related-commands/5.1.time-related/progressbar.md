### progressbar
```
*progressbar "<color>",<seconds>;
```

This command works almost like sleep2, but displays a progress bar
above the head of the currently attached character (like cast bar).
Once the given amount of seconds passes, the script resumes. If the
character moves while the progress bar progresses, it is aborted and
the script ends. The color format is in RGB (RRGGBB). The color is
currently ignored by the client and appears always green.
