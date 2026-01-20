# 2025 Book Reviews
```dataview
table author, rating, category
from #book
where contains(status, "read") AND !contains(status, "unread") AND !contains(status, "reading")
where !contains(file.path, "1 - Reading Lists")
where !contains(file.path, "Templates")
where date_finished.year = 2025
sort rating desc
```
