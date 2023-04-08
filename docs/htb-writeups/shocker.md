```python
def greeting(name):
    print("Hello, " + name + "!")
    
greeting("Alice")
```
| Column 1 | Column 2 |
| -------- | -------- |
| Row 1, Column 1 | Row 1, Column 2 |
| Row 2, Column 1 | Row 2, Column 2 |

<style>
table {
  width: 90%;
  border: 1px solid black;
  border-collapse: collapse;
}

th, td {
  padding: 10px;
  text-align: center;
  border: 1px solid black;
}
</style>

| Column 1 | Column 2 |
| -------- | -------- |
| <span style="color: white;">This text is red</span> | Row 1, Column 2 |
| Row 2, Column 1 | Row 2, Column 2 |


<br>

<style>
.columns {
  display: flex;
}
.column {
  flex: 1;
  padding: 0 10px;
}
</style>

<div class="columns">
  <div class="column">
    - Item 1
    - Item 2
    - Item 3
  </div>
  <div class="column">
    - Item 4
    - Item 5
    - Item 6
  </div>
</div>
