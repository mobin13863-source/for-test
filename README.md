"""This code is a Task Manager program."""

"""its a small app that helps you organize and track your daily tasks."""

import os
import json
import random
import datetime
import re
from collections import Counter


Deta_file = "task.json"

"""loading data"""


def load_tasks():
    if os.path.exists(Deta_file):
        try:
            with open(Deta_file, "r", encoding="utf-8") as f:
                return json.load(f)
        except json.JSONDecodeError:
            print("The file is corrupted, the empty list has been fixed.")
            return []
    return []


"""Data storage"""


def save_tasks(tasks):
    with open(Deta_file, "w", encoding="utf-8") as f:
        json.dump(tasks, f, ensure_ascii=False, indent=4)


"""make class"""


class Task:
    def __init__(self, title, description, duo_date):
        self.id = random.randint(1_000, 9_999)
        self.title = title
        self.description = description
        self.created = datetime.date.today().isoformat()
        self.duo_date = duo_date
        self.status = "not_done"

    def to_dick(self):
        return {
            "id": self.id,
            "title": self.title,
            "description": self.description,
            "creat": self.created,
            "duo_date": self.duo_date,
            "status": self.status,
        }

    """do opretion"""

def add_task(tasks):
    title = input("Title of work: ").strip()
    description = input("desctiptions of work: ").strip()
    duo_date = input("promised (YYYY-MM-DD): ").strip()

    pattern = re.compile(
        r"\d{4}-\d{2}-\d{2}"
    )  # I write the pattern for the re.match
    if not re.match(pattern, duo_date):  # the re.match for comparison
        print("the date is not valid")
        return

    new_task = Task(title, description, duo_date)
    tasks.append(new_task.to_dick())
    save_tasks(tasks)
    print("the add is success")

def show_task(task):
    if not task:
        print("It is empty")
        return
    for i in task:
        print(
            f"{i['id']}, {i['title']}, {i['description']}, {i['status']}, {i['duo_date']},"
        )

def search_task(task):
    Keyword = input("give me title you wnat").strip().lower()
    result = [t for t in task if Keyword in t["title"].lower()]
    if result:
        for r in result:
            print(f"{r['id']}, {r['title']}, {r['status']}")
    else:
        print("Nothing was found.")

def change_status(task):
    try:
        task_id = int(input("wrtite the id"))
    except ValueError:
        print("The ID is invalid.")
        return
    for t in task:
        if t["id"] == task_id:
            t["status"] = "done" if t["status"] == "not_done" else "not_done"
            print("change is success")
            return
    print("nothong was found")

def delet_task(task):
    try:
        task_id = int(input("write the id"))
    except ValueError:
        print("The ID is invalid.")
        return
    
    for t in task:
        if t["id"] == task_id:
            task.remove(t)
            print("the delet is success")
            return
    print("nothing was found")

def report_task(task):
    statuses = Counter(t["status"] for t in task)
    print("Report")
    print("done:", statuses.get("done", 0))
    print("not done:", statuses.get("not done", 0))

"""the mino"""

def mino():
    task = load_tasks()
    while True:
        print("\n--- mino --- ")
        print("1. add")
        print("2. show")
        print("3. sereach")
        print("4. change")
        print("5. delet")
        print("6. report")
        print("7 exit")

        choies = input("choies:").strip()
        if choies == "1":
            add_task(task)
        elif choies == "2":
            show_task(task)
        elif choies == "3":
            search_task(task)
        elif choies == "4":
            change_status(task)
        elif choies == "5":
            delet_task(task)
        elif choies == "6":
            report_task(task)
        elif choies == "7":
            print("nive to meet you")
            break
        else:
            print("choies one of them")
            continue

if __name__ == "__main__":
    mino()
else:
    print("use me as pachage")
    
