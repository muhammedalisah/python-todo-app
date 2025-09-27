# python-todo-app
README.mdd
# Python To-Do List App
Basit bir komut satırı yapılacaklar listesi uygulaması.

## Kullanım
python todo.py add "alışveriş yap"
python todo.py list
python todo.py remove 1

## Gereksinimler
- Python 3.x
import sys
import json
from pathlib import Path

DATA_FILE = Path("tasks.json")

def load_tasks():
    if DATA_FILE.exists():
        return json.loads(DATA_FILE.read_text())
    return []

def save_tasks(tasks):
    DATA_FILE.write_text(json.dumps(tasks, ensure_ascii=False, indent=2))

def add_task(task):
    tasks = load_tasks()
    tasks.append(task)
    save_tasks(tasks)
    print(f"Görev eklendi: {task}")

def list_tasks():
    tasks = load_tasks()
    if not tasks:
        print("Henüz görev yok.")
    else:
        for i, task in enumerate(tasks, start=1):
            print(f"{i}. {task}")

def remove_task(index):
    tasks = load_tasks()
    try:
        removed = tasks.pop(index - 1)
        save_tasks(tasks)
        print(f"Görev silindi: {removed}")
    except:
        print("Geçersiz görev numarası.")

if __name__ == "__main__":
    if len(sys.argv) < 2:
        print("Kullanım: python todo.py [add/list/remove] [görev]")
    else:
        cmd = sys.argv[1]
        if cmd == "add":
            add_task(" ".join(sys.argv[2:]))
        elif cmd == "list":
            list_tasks()
        elif cmd == "remove":
            remove_task(int(sys.argv[2]))
        else:
            print("Bilinmeyen komut.")
