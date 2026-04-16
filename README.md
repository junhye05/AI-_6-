# AI 디지털 전환_6주차 과제
---

<br>

## 실습1 : Claude 시작하기와 첫 대화
<img width="766" height="699" alt="image" src="https://github.com/user-attachments/assets/51faa984-6254-4d54-80f2-f8985e2a55e4" />
<img width="725" height="260" alt="image" src="https://github.com/user-attachments/assets/65faa945-24d8-4314-8bea-b58575c83b54" />


## 실습 2 

### [긴 문서 완벽하게 요약하기]
<img width="745" height="628" alt="image" src="https://github.com/user-attachments/assets/e95c5238-cc8d-41a4-a25f-8ee0d9b445fe" />
<img width="739" height="633" alt="image" src="https://github.com/user-attachments/assets/748cda08-55b8-40a1-8517-7897e6060835" />

---

<br>

### [심화 : 문서 기반 심층 질의응답]
<img width="736" height="651" alt="image" src="https://github.com/user-attachments/assets/ba89f79b-202d-4a45-82a6-2f11378a374a" />
<img width="737" height="484" alt="image" src="https://github.com/user-attachments/assets/43163184-0354-40c0-bcd3-01cc24a7889b" />
<img width="746" height="650" alt="image" src="https://github.com/user-attachments/assets/e6e84c4f-ae10-46d8-8a5a-539b3bcc8ade" />
<img width="726" height="660" alt="image" src="https://github.com/user-attachments/assets/1642dcf6-2601-42c6-a652-650d2ca6ca69" />
<img width="740" height="412" alt="image" src="https://github.com/user-attachments/assets/0b3e9fb6-0fea-4a79-95b4-228ce9de8db8" />

---

<br>
<br>

## 실습3 

### [조건이 부여된 복잡한 코드 생성]
<img width="762" height="363" alt="image" src="https://github.com/user-attachments/assets/dc248a91-b515-4c75-a1f3-96315795f2e9" />
<img width="739" height="695" alt="image" src="https://github.com/user-attachments/assets/4afd630e-d82b-4b0c-87af-1e3a28f39506" />

```
"""
============================================================
  할 일 관리 프로그램 (TO DO List)
  작성 언어 : Python 3
  주요 기능 : 추가 / 목록 보기 / 완료 표시 / 삭제 / JSON 저장·불러오기
============================================================
"""

import json       # JSON 파일 읽기/쓰기를 위한 내장 모듈
import os         # 파일 존재 여부 확인을 위한 내장 모듈

# ─────────────────────────────────────────
# 상수 정의
# ─────────────────────────────────────────
FILE_NAME = "todos.json"   # 할 일 목록을 저장할 JSON 파일 이름


# ─────────────────────────────────────────
# 1. JSON 파일에서 할 일 목록 불러오기
# ─────────────────────────────────────────
def load_todos():
    """
    저장된 JSON 파일에서 할 일 목록을 불러옵니다.
    파일이 없으면 빈 리스트를 반환합니다.

    반환값(return):
        list: 할 일 딕셔너리들의 리스트
              예) [{"id": 1, "task": "운동하기", "done": False}, ...]
    """
    # 파일이 존재하는지 먼저 확인
    if os.path.exists(FILE_NAME):
        try:
            with open(FILE_NAME, "r", encoding="utf-8") as f:
                return json.load(f)   # JSON → 파이썬 리스트로 변환
        except (json.JSONDecodeError, IOError) as e:
            # 파일이 손상되었거나 읽기 오류가 발생한 경우
            print(f"  ⚠️  파일 불러오기 오류: {e}")
            return []
    else:
        # 파일이 없으면 새 목록으로 시작
        return []


# ─────────────────────────────────────────
# 2. JSON 파일에 할 일 목록 저장하기
# ─────────────────────────────────────────
def save_todos(todos):
    """
    현재 할 일 목록을 JSON 파일에 저장합니다.

    매개변수(parameter):
        todos (list): 저장할 할 일 목록 리스트
    """
    try:
        with open(FILE_NAME, "w", encoding="utf-8") as f:
            # indent=2 → 사람이 읽기 쉽게 들여쓰기 2칸으로 저장
            # ensure_ascii=False → 한글이 깨지지 않도록 설정
            json.dump(todos, f, indent=2, ensure_ascii=False)
        print(f"  💾  '{FILE_NAME}' 파일에 저장되었습니다.")
    except IOError as e:
        print(f"  ⚠️  파일 저장 오류: {e}")


# ─────────────────────────────────────────
# 3. 할 일 추가하기
# ─────────────────────────────────────────
def add_todo(todos):
    """
    새로운 할 일을 목록에 추가합니다.

    매개변수(parameter):
        todos (list): 현재 할 일 목록 리스트 (직접 수정됨)
    """
    print("\n  [ 할 일 추가 ]")
    task = input("  추가할 할 일을 입력하세요: ").strip()

    # 빈 문자열 입력 방지
    if not task:
        print("  ⚠️  할 일 내용을 입력해 주세요.")
        return

    # 새 할 일 딕셔너리 생성
    # id: 목록이 비어 있으면 1, 아니면 마지막 항목의 id + 1
    new_id = todos[-1]["id"] + 1 if todos else 1
    new_todo = {
        "id"  : new_id,    # 고유 번호
        "task": task,       # 할 일 내용
        "done": False       # 완료 여부 (기본값: 미완료)
    }

    todos.append(new_todo)   # 목록에 추가
    save_todos(todos)        # 파일에 저장
    print(f"  ✅  '{task}' 이(가) 추가되었습니다! (번호: {new_id})")


# ─────────────────────────────────────────
# 4. 할 일 목록 보기
# ─────────────────────────────────────────
def view_todos(todos):
    """
    현재 할 일 목록을 화면에 출력합니다.

    매개변수(parameter):
        todos (list): 현재 할 일 목록 리스트
    """
    print("\n  [ 할 일 목록 ]")
    print("  " + "─" * 40)

    # 목록이 비어 있는 경우
    if not todos:
        print("  📭  등록된 할 일이 없습니다.")
        print("  " + "─" * 40)
        return

    # 각 할 일 항목 출력
    for todo in todos:
        # 완료 여부에 따라 아이콘 변경
        status = "✔ 완료" if todo["done"] else "○ 미완료"
        # 완료된 항목은 취소선 느낌으로 표시
        task_display = f"[{todo['task']}]" if todo["done"] else todo["task"]
        print(f"  {todo['id']:>3}. [{status}]  {task_display}")

    print("  " + "─" * 40)
    # 완료/전체 통계 출력
    done_count = sum(1 for t in todos if t["done"])
    print(f"  📊  전체: {len(todos)}개 | 완료: {done_count}개 | 미완료: {len(todos) - done_count}개")


# ─────────────────────────────────────────
# 5. 할 일 완료 표시하기
# ─────────────────────────────────────────
def complete_todo(todos):
    """
    특정 번호의 할 일을 완료 상태로 변경합니다.

    매개변수(parameter):
        todos (list): 현재 할 일 목록 리스트 (직접 수정됨)
    """
    print("\n  [ 완료 표시 ]")
    view_todos(todos)   # 현재 목록을 먼저 보여줌

    if not todos:
        return

    try:
        todo_id = int(input("  완료할 항목의 번호를 입력하세요: "))
    except ValueError:
        # 숫자가 아닌 값 입력 시 예외 처리
        print("  ⚠️  올바른 번호를 입력해 주세요.")
        return

    # id가 일치하는 항목 찾기
    for todo in todos:
        if todo["id"] == todo_id:
            if todo["done"]:
                print(f"  ℹ️   '{todo['task']}' 은(는) 이미 완료된 항목입니다.")
            else:
                todo["done"] = True   # 완료 상태로 변경
                save_todos(todos)
                print(f"  🎉  '{todo['task']}' 을(를) 완료 처리했습니다!")
            return

    # 해당 번호가 없는 경우
    print(f"  ⚠️  번호 {todo_id}에 해당하는 항목이 없습니다.")


# ─────────────────────────────────────────
# 6. 할 일 삭제하기
# ─────────────────────────────────────────
def delete_todo(todos):
    """
    특정 번호의 할 일을 목록에서 삭제합니다.

    매개변수(parameter):
        todos (list): 현재 할 일 목록 리스트 (직접 수정됨)
    """
    print("\n  [ 할 일 삭제 ]")
    view_todos(todos)   # 현재 목록을 먼저 보여줌

    if not todos:
        return

    try:
        todo_id = int(input("  삭제할 항목의 번호를 입력하세요: "))
    except ValueError:
        print("  ⚠️  올바른 번호를 입력해 주세요.")
        return

    # id가 일치하는 항목 찾기
    for todo in todos:
        if todo["id"] == todo_id:
            # 실수로 삭제하지 않도록 재확인
            confirm = input(f"  정말로 '{todo['task']}' 을(를) 삭제할까요? (y/n): ").strip().lower()
            if confirm == "y":
                todos.remove(todo)   # 목록에서 제거
                save_todos(todos)
                print(f"  🗑️   '{todo['task']}' 이(가) 삭제되었습니다.")
            else:
                print("  ↩️   삭제를 취소했습니다.")
            return

    print(f"  ⚠️  번호 {todo_id}에 해당하는 항목이 없습니다.")


# ─────────────────────────────────────────
# 7. 메인 메뉴 출력 함수
# ─────────────────────────────────────────
def print_menu():
    """
    사용자에게 메인 메뉴를 출력합니다.
    """
    print("\n" + "=" * 44)
    print("       📝  할 일 관리 프로그램 (TO DO)")
    print("=" * 44)
    print("  1️⃣   할 일 추가")
    print("  2️⃣   할 일 목록 보기")
    print("  3️⃣   완료 표시")
    print("  4️⃣   할 일 삭제")
    print("  0️⃣   프로그램 종료")
    print("=" * 44)


# ─────────────────────────────────────────
# 8. 메인 실행 함수 (프로그램 진입점)
# ─────────────────────────────────────────
def main():
    """
    프로그램의 메인 루프입니다.
    사용자 입력을 받아 해당 기능을 실행합니다.
    """
    print("\n  🚀  프로그램을 시작합니다...")
    todos = load_todos()   # 프로그램 시작 시 저장된 목록 불러오기

    if todos:
        print(f"  📂  저장된 할 일 {len(todos)}개를 불러왔습니다.")

    # 무한 루프: 사용자가 0을 입력할 때까지 반복
    while True:
        print_menu()   # 메뉴 출력

        choice = input("  원하는 메뉴 번호를 입력하세요: ").strip()

        # 입력값에 따라 해당 함수 호출
        if choice == "1":
            add_todo(todos)
        elif choice == "2":
            view_todos(todos)
        elif choice == "3":
            complete_todo(todos)
        elif choice == "4":
            delete_todo(todos)
        elif choice == "0":
            print("\n  👋  프로그램을 종료합니다. 수고하셨습니다!\n")
            break   # 루프 탈출 → 프로그램 종료
        else:
            # 1, 2, 3, 4, 0 이외의 값 입력 시
            print("  ⚠️  올바른 메뉴 번호(0~4)를 입력해 주세요.")


# ─────────────────────────────────────────
# 프로그램 시작점
# 이 파일을 직접 실행할 때만 main() 호출
# (다른 파일에서 import 했을 때는 실행 안 됨)
# ─────────────────────────────────────────
if __name__ == "__main__":
    main()
```

<br>

---

<br>

```
"""
============================================================
  할 일 관리 프로그램 (TO DO List)
  작성 언어 : Python 3
  주요 기능 : 추가 / 목록 보기 / 완료 표시 / 삭제 / JSON 저장·불러오기
============================================================
"""

import json       # JSON 파일 읽기/쓰기를 위한 내장 모듈
import os         # 파일 존재 여부 확인을 위한 내장 모듈

# ─────────────────────────────────────────
# 상수 정의
# ─────────────────────────────────────────
FILE_NAME = "todos.json"   # 할 일 목록을 저장할 JSON 파일 이름


# ─────────────────────────────────────────
# 1. JSON 파일에서 할 일 목록 불러오기
# ─────────────────────────────────────────
def load_todos():
    """
    저장된 JSON 파일에서 할 일 목록을 불러옵니다.
    파일이 없으면 빈 리스트를 반환합니다.

    반환값(return):
        list: 할 일 딕셔너리들의 리스트
              예) [{"id": 1, "task": "운동하기", "done": False}, ...]
    """
    # 파일이 존재하는지 먼저 확인
    if os.path.exists(FILE_NAME):
        try:
            with open(FILE_NAME, "r", encoding="utf-8") as f:
                return json.load(f)   # JSON → 파이썬 리스트로 변환
        except (json.JSONDecodeError, IOError) as e:
            # 파일이 손상되었거나 읽기 오류가 발생한 경우
            print(f"  ⚠️  파일 불러오기 오류: {e}")
            return []
    else:
        # 파일이 없으면 새 목록으로 시작
        return []


# ─────────────────────────────────────────
# 2. JSON 파일에 할 일 목록 저장하기
# ─────────────────────────────────────────
def save_todos(todos):
    """
    현재 할 일 목록을 JSON 파일에 저장합니다.

    매개변수(parameter):
        todos (list): 저장할 할 일 목록 리스트
    """
    try:
        with open(FILE_NAME, "w", encoding="utf-8") as f:
            # indent=2 → 사람이 읽기 쉽게 들여쓰기 2칸으로 저장
            # ensure_ascii=False → 한글이 깨지지 않도록 설정
            json.dump(todos, f, indent=2, ensure_ascii=False)
        print(f"  💾  '{FILE_NAME}' 파일에 저장되었습니다.")
    except IOError as e:
        print(f"  ⚠️  파일 저장 오류: {e}")


# ─────────────────────────────────────────
# 3. 할 일 추가하기
# ─────────────────────────────────────────
def add_todo(todos):
    """
    새로운 할 일을 목록에 추가합니다.

    매개변수(parameter):
        todos (list): 현재 할 일 목록 리스트 (직접 수정됨)
    """
    print("\n  [ 할 일 추가 ]")
    task = input("  추가할 할 일을 입력하세요: ").strip()

    # 빈 문자열 입력 방지
    if not task:
        print("  ⚠️  할 일 내용을 입력해 주세요.")
        return

    # 새 할 일 딕셔너리 생성
    # id: 목록이 비어 있으면 1, 아니면 마지막 항목의 id + 1
    new_id = todos[-1]["id"] + 1 if todos else 1
    new_todo = {
        "id"  : new_id,    # 고유 번호
        "task": task,       # 할 일 내용
        "done": False       # 완료 여부 (기본값: 미완료)
    }

    todos.append(new_todo)   # 목록에 추가
    save_todos(todos)        # 파일에 저장
    print(f"  ✅  '{task}' 이(가) 추가되었습니다! (번호: {new_id})")


# ─────────────────────────────────────────
# 4. 할 일 목록 보기
# ─────────────────────────────────────────
def view_todos(todos):
    """
    현재 할 일 목록을 화면에 출력합니다.

    매개변수(parameter):
        todos (list): 현재 할 일 목록 리스트
    """
    print("\n  [ 할 일 목록 ]")
    print("  " + "─" * 40)

    # 목록이 비어 있는 경우
    if not todos:
        print("  📭  등록된 할 일이 없습니다.")
        print("  " + "─" * 40)
        return

    # 각 할 일 항목 출력
    for todo in todos:
        # 완료 여부에 따라 아이콘 변경
        status = "✔ 완료" if todo["done"] else "○ 미완료"
        # 완료된 항목은 취소선 느낌으로 표시
        task_display = f"[{todo['task']}]" if todo["done"] else todo["task"]
        print(f"  {todo['id']:>3}. [{status}]  {task_display}")

    print("  " + "─" * 40)
    # 완료/전체 통계 출력
    done_count = sum(1 for t in todos if t["done"])
    print(f"  📊  전체: {len(todos)}개 | 완료: {done_count}개 | 미완료: {len(todos) - done_count}개")


# ─────────────────────────────────────────
# 5. 할 일 완료 표시하기
# ─────────────────────────────────────────
def complete_todo(todos):
    """
    특정 번호의 할 일을 완료 상태로 변경합니다.

    매개변수(parameter):
        todos (list): 현재 할 일 목록 리스트 (직접 수정됨)
    """
    print("\n  [ 완료 표시 ]")
    view_todos(todos)   # 현재 목록을 먼저 보여줌

    if not todos:
        return

    try:
        todo_id = int(input("  완료할 항목의 번호를 입력하세요: "))
    except ValueError:
        # 숫자가 아닌 값 입력 시 예외 처리
        print("  ⚠️  올바른 번호를 입력해 주세요.")
        return

    # id가 일치하는 항목 찾기
    for todo in todos:
        if todo["id"] == todo_id:
            if todo["done"]:
                print(f"  ℹ️   '{todo['task']}' 은(는) 이미 완료된 항목입니다.")
            else:
                todo["done"] = True   # 완료 상태로 변경
                save_todos(todos)
                print(f"  🎉  '{todo['task']}' 을(를) 완료 처리했습니다!")
            return

    # 해당 번호가 없는 경우
    print(f"  ⚠️  번호 {todo_id}에 해당하는 항목이 없습니다.")


# ─────────────────────────────────────────
# 6. 할 일 삭제하기
# ─────────────────────────────────────────
def delete_todo(todos):
    """
    특정 번호의 할 일을 목록에서 삭제합니다.

    매개변수(parameter):
        todos (list): 현재 할 일 목록 리스트 (직접 수정됨)
    """
    print("\n  [ 할 일 삭제 ]")
    view_todos(todos)   # 현재 목록을 먼저 보여줌

    if not todos:
        return

    try:
        todo_id = int(input("  삭제할 항목의 번호를 입력하세요: "))
    except ValueError:
        print("  ⚠️  올바른 번호를 입력해 주세요.")
        return

    # id가 일치하는 항목 찾기
    for todo in todos:
        if todo["id"] == todo_id:
            # 실수로 삭제하지 않도록 재확인
            confirm = input(f"  정말로 '{todo['task']}' 을(를) 삭제할까요? (y/n): ").strip().lower()
            if confirm == "y":
                todos.remove(todo)   # 목록에서 제거
                save_todos(todos)
                print(f"  🗑️   '{todo['task']}' 이(가) 삭제되었습니다.")
            else:
                print("  ↩️   삭제를 취소했습니다.")
            return

    print(f"  ⚠️  번호 {todo_id}에 해당하는 항목이 없습니다.")


# ─────────────────────────────────────────
# 7. 메인 메뉴 출력 함수
# ─────────────────────────────────────────
def print_menu():
    """
    사용자에게 메인 메뉴를 출력합니다.
    """
    print("\n" + "=" * 44)
    print("       📝  할 일 관리 프로그램 (TO DO)")
    print("=" * 44)
    print("  1️⃣   할 일 추가")
    print("  2️⃣   할 일 목록 보기")
    print("  3️⃣   완료 표시")
    print("  4️⃣   할 일 삭제")
    print("  0️⃣   프로그램 종료")
    print("=" * 44)


# ─────────────────────────────────────────
# 8. 메인 실행 함수 (프로그램 진입점)
# ─────────────────────────────────────────
def main():
    """
    프로그램의 메인 루프입니다.
    사용자 입력을 받아 해당 기능을 실행합니다.
    """
    print("\n  🚀  프로그램을 시작합니다...")
    todos = load_todos()   # 프로그램 시작 시 저장된 목록 불러오기

    if todos:
        print(f"  📂  저장된 할 일 {len(todos)}개를 불러왔습니다.")

    # 무한 루프: 사용자가 0을 입력할 때까지 반복
    while True:
        print_menu()   # 메뉴 출력

        choice = input("  원하는 메뉴 번호를 입력하세요: ").strip()

        # 입력값에 따라 해당 함수 호출
        if choice == "1":
            add_todo(todos)
        elif choice == "2":
            view_todos(todos)
        elif choice == "3":
            complete_todo(todos)
        elif choice == "4":
            delete_todo(todos)
        elif choice == "0":
            print("\n  👋  프로그램을 종료합니다. 수고하셨습니다!\n")
            break   # 루프 탈출 → 프로그램 종료
        else:
            # 1, 2, 3, 4, 0 이외의 값 입력 시
            print("  ⚠️  올바른 메뉴 번호(0~4)를 입력해 주세요.")


# ─────────────────────────────────────────
# 프로그램 시작점
# 이 파일을 직접 실행할 때만 main() 호출
# (다른 파일에서 import 했을 때는 실행 안 됨)
# ─────────────────────────────────────────
if __name__ == "__main__":
    main()
```


---

<br>

### [심화 : 시니어 개발자 수준의 코드 리뷰]

#### {Misson 1: 갹체지향(OOP) 리팩토링 요청}
<img width="747" height="519" alt="image" src="https://github.com/user-attachments/assets/25fd181b-3070-41c2-a4ce-c3898441de5f" />
<img width="749" height="595" alt="image" src="https://github.com/user-attachments/assets/22f3c1e6-8659-4f17-9cdd-ae319359a1a7" />
<img width="781" height="669" alt="image" src="https://github.com/user-attachments/assets/0b2584f7-1347-4278-b1ac-5d7357843a26" />

```
"""
============================================================
  할 일 관리 프로그램 (TO DO List) - OOP 리팩토링 버전
  작성 언어 : Python 3
  설계 방식 : 객체지향 프로그래밍 (Object-Oriented Programming)

  클래스 구조:
    ┌─────────────────────────────────┐
    │  TodoItem                       │  ← 할 일 '데이터' 하나를 표현
    ├─────────────────────────────────┤
    │  TodoStorage                    │  ← JSON 파일 입출력 담당
    ├─────────────────────────────────┤
    │  TodoManager                    │  ← 할 일 목록 비즈니스 로직 담당
    ├─────────────────────────────────┤
    │  TodoApp                        │  ← UI 메뉴 및 사용자 인터랙션 담당
    └─────────────────────────────────┘
============================================================
"""

import json
import os


# ╔══════════════════════════════════════════════════════════╗
# ║  CLASS 1 : TodoItem                                      ║
# ║  역할 : 할 일 항목 하나의 데이터 구조를 정의하는 클래스  ║
# ║  (데이터 모델 / Model Layer)                             ║
# ╚══════════════════════════════════════════════════════════╝
class TodoItem:
    """
    할 일 항목 하나를 표현하는 클래스.

    속성(Attributes):
        id   (int)  : 항목의 고유 번호
        task (str)  : 할 일 내용
        done (bool) : 완료 여부 (True=완료, False=미완료)
    """

    def __init__(self, id: int, task: str, done: bool = False):
        """
        TodoItem 객체를 생성(초기화)하는 생성자 메서드.

        매개변수:
            id   (int)  : 고유 번호
            task (str)  : 할 일 내용
            done (bool) : 완료 여부 (기본값: False)
        """
        self.id   = id
        self.task = task
        self.done = done

    def complete(self):
        """할 일을 완료 상태로 변경합니다."""
        self.done = True

    def to_dict(self) -> dict:
        """
        객체를 딕셔너리로 변환합니다. (JSON 저장 시 사용)

        반환값:
            dict: {"id": ..., "task": ..., "done": ...}
        """
        return {
            "id"  : self.id,
            "task": self.task,
            "done": self.done
        }

    @classmethod
    def from_dict(cls, data: dict) -> "TodoItem":
        """
        딕셔너리에서 TodoItem 객체를 생성합니다. (JSON 불러올 때 사용)

        @classmethod : 인스턴스 없이 클래스 이름으로 바로 호출 가능
        예) TodoItem.from_dict({"id": 1, "task": "운동", "done": False})

        매개변수:
            data (dict): JSON에서 읽어온 딕셔너리
        반환값:
            TodoItem: 새로운 TodoItem 객체
        """
        return cls(
            id   = data["id"],
            task = data["task"],
            done = data.get("done", False)   # "done" 키가 없으면 False
        )

    def __str__(self) -> str:
        """
        print(todo_item) 호출 시 보여줄 문자열을 정의합니다.
        __str__은 파이썬의 특수 메서드(매직 메서드)입니다.
        """
        status       = "✔ 완료 " if self.done else "○ 미완료"
        task_display = f"[{self.task}]" if self.done else self.task
        return f"  {self.id:>3}. [{status}]  {task_display}"


# ╔══════════════════════════════════════════════════════════╗
# ║  CLASS 2 : TodoStorage                                   ║
# ║  역할 : JSON 파일 저장 / 불러오기 전담 클래스            ║
# ║  (데이터 접근 계층 / Data Access Layer)                  ║
# ╚══════════════════════════════════════════════════════════╝
class TodoStorage:
    """
    JSON 파일 입출력을 전담하는 클래스.
    파일 관련 로직을 별도 클래스로 분리하면,
    나중에 DB나 클라우드 저장소로 교체할 때 이 클래스만 수정하면 됩니다.
    → OOP의 핵심 원칙 중 '단일 책임 원칙(SRP)' 적용
    """

    def __init__(self, file_name: str = "todos.json"):
        """
        매개변수:
            file_name (str): 저장할 JSON 파일 경로 (기본값: todos.json)
        """
        self.file_name = file_name

    def load(self) -> list:
        """
        JSON 파일에서 할 일 목록을 불러와 TodoItem 객체 리스트로 반환합니다.

        반환값:
            list[TodoItem]: TodoItem 객체들의 리스트
        """
        if not os.path.exists(self.file_name):
            return []   # 파일 없으면 빈 리스트 반환

        try:
            with open(self.file_name, "r", encoding="utf-8") as f:
                raw_list = json.load(f)   # JSON → 파이썬 딕셔너리 리스트
            # 각 딕셔너리를 TodoItem 객체로 변환 (리스트 컴프리헨션)
            return [TodoItem.from_dict(item) for item in raw_list]
        except (json.JSONDecodeError, KeyError, IOError) as e:
            print(f"  ⚠️  파일 불러오기 오류: {e}")
            return []

    def save(self, todos: list) -> bool:
        """
        TodoItem 객체 리스트를 JSON 파일에 저장합니다.

        매개변수:
            todos (list[TodoItem]): 저장할 TodoItem 리스트
        반환값:
            bool: 저장 성공 여부
        """
        try:
            with open(self.file_name, "w", encoding="utf-8") as f:
                # 각 TodoItem 객체를 딕셔너리로 변환 후 JSON 저장
                json.dump(
                    [todo.to_dict() for todo in todos],
                    f,
                    indent=2,
                    ensure_ascii=False
                )
            return True
        except IOError as e:
            print(f"  ⚠️  파일 저장 오류: {e}")
            return False


# ╔══════════════════════════════════════════════════════════╗
# ║  CLASS 3 : TodoManager                                   ║
# ║  역할 : 할 일 목록의 비즈니스 로직 담당 클래스           ║
# ║  (비즈니스 로직 계층 / Business Logic Layer)             ║
# ╚══════════════════════════════════════════════════════════╝
class TodoManager:
    """
    할 일 목록의 핵심 로직(추가·조회·완료·삭제)을 담당하는 클래스.
    TodoStorage를 내부적으로 사용하여 데이터를 영구 저장합니다.
    → OOP의 '합성(Composition)' 패턴 적용
    """

    def __init__(self, storage: TodoStorage):
        """
        매개변수:
            storage (TodoStorage): 파일 입출력 담당 객체
                                   (외부에서 주입 → 의존성 주입 패턴)
        """
        self.storage = storage
        self.todos   = self.storage.load()   # 시작 시 파일에서 불러오기

    def _get_next_id(self) -> int:
        """
        새 항목에 부여할 다음 ID를 계산합니다.
        (내부에서만 사용하는 메서드 → 이름 앞에 _ 붙임, 파이썬 관례)

        반환값:
            int: 마지막 ID + 1 (목록이 비면 1)
        """
        return self.todos[-1].id + 1 if self.todos else 1

    def _find_by_id(self, todo_id: int) -> TodoItem | None:
        """
        ID로 TodoItem을 찾아 반환합니다.

        매개변수:
            todo_id (int): 찾을 항목의 ID
        반환값:
            TodoItem 또는 None (없으면 None)
        """
        for todo in self.todos:
            if todo.id == todo_id:
                return todo
        return None

    def add(self, task: str) -> TodoItem:
        """
        새 할 일을 추가하고 저장합니다.

        매개변수:
            task (str): 추가할 할 일 내용
        반환값:
            TodoItem: 새로 생성된 객체
        """
        new_todo = TodoItem(id=self._get_next_id(), task=task)
        self.todos.append(new_todo)
        self.storage.save(self.todos)
        return new_todo

    def get_all(self) -> list:
        """전체 할 일 목록을 반환합니다."""
        return self.todos

    def complete(self, todo_id: int) -> TodoItem | None:
        """
        특정 ID의 할 일을 완료 처리합니다.

        반환값:
            TodoItem : 완료 처리된 객체
            None     : 해당 ID 없음
        """
        todo = self._find_by_id(todo_id)
        if todo:
            todo.complete()   # TodoItem의 complete() 메서드 호출
            self.storage.save(self.todos)
        return todo

    def delete(self, todo_id: int) -> TodoItem | None:
        """
        특정 ID의 할 일을 삭제합니다.

        반환값:
            TodoItem : 삭제된 객체
            None     : 해당 ID 없음
        """
        todo = self._find_by_id(todo_id)
        if todo:
            self.todos.remove(todo)
            self.storage.save(self.todos)
        return todo

    def get_stats(self) -> dict:
        """
        할 일 통계를 딕셔너리로 반환합니다.

        반환값:
            dict: {"total": N, "done": N, "pending": N}
        """
        total   = len(self.todos)
        done    = sum(1 for t in self.todos if t.done)
        pending = total - done
        return {"total": total, "done": done, "pending": pending}


# ╔══════════════════════════════════════════════════════════╗
# ║  CLASS 4 : TodoApp                                       ║
# ║  역할 : 메뉴 출력 및 사용자 입력 처리 담당 클래스        ║
# ║  (UI / Presentation Layer)                               ║
# ╚══════════════════════════════════════════════════════════╝
class TodoApp:
    """
    사용자와 상호작용하는 UI 계층 클래스.
    TodoManager를 통해서만 데이터에 접근합니다.
    → UI 로직과 비즈니스 로직이 완전히 분리됨
    """

    def __init__(self, manager: TodoManager):
        """
        매개변수:
            manager (TodoManager): 비즈니스 로직 담당 객체
        """
        self.manager = manager

    # ── 내부 헬퍼 메서드들 ──────────────────────────────────

    def _print_divider(self, char: str = "─", width: int = 44):
        """구분선을 출력합니다."""
        print("  " + char * width)

    def _input_int(self, prompt: str) -> int | None:
        """
        정수 입력을 받고, 변환 실패 시 None을 반환합니다.
        예외 처리를 한 곳에 모아 코드 중복을 제거합니다.
        """
        try:
            return int(input(prompt).strip())
        except ValueError:
            print("  ⚠️  숫자를 입력해 주세요.")
            return None

    # ── 기능별 메서드들 ──────────────────────────────────────

    def show_menu(self):
        """메인 메뉴를 출력합니다."""
        print("\n" + "=" * 46)
        print("       📝  할 일 관리 프로그램 (OOP ver.)")
        print("=" * 46)
        print("  1️⃣   할 일 추가")
        print("  2️⃣   할 일 목록 보기")
        print("  3️⃣   완료 표시")
        print("  4️⃣   할 일 삭제")
        print("  0️⃣   프로그램 종료")
        print("=" * 46)

    def handle_add(self):
        """할 일 추가 UI를 처리합니다."""
        print("\n  [ 할 일 추가 ]")
        task = input("  추가할 할 일을 입력하세요: ").strip()

        if not task:
            print("  ⚠️  내용을 입력해 주세요.")
            return

        new_todo = self.manager.add(task)
        print(f"  ✅  '{new_todo.task}' 추가 완료! (번호: {new_todo.id})")
        print(f"  💾  파일에 자동 저장되었습니다.")

    def handle_view(self):
        """할 일 목록 보기 UI를 처리합니다."""
        print("\n  [ 할 일 목록 ]")
        self._print_divider()

        todos = self.manager.get_all()

        if not todos:
            print("  📭  등록된 할 일이 없습니다.")
        else:
            # TodoItem의 __str__ 메서드가 자동으로 호출됨
            for todo in todos:
                print(todo)

        self._print_divider()

        # 통계 출력
        stats = self.manager.get_stats()
        print(f"  📊  전체: {stats['total']}개 | "
              f"완료: {stats['done']}개 | "
              f"미완료: {stats['pending']}개")

    def handle_complete(self):
        """완료 표시 UI를 처리합니다."""
        print("\n  [ 완료 표시 ]")
        self.handle_view()

        if not self.manager.get_all():
            return

        todo_id = self._input_int("  완료할 항목 번호: ")
        if todo_id is None:
            return

        todo = self.manager.complete(todo_id)

        if todo is None:
            print(f"  ⚠️  번호 {todo_id}에 해당하는 항목이 없습니다.")
        else:
            print(f"  🎉  '{todo.task}' 완료 처리되었습니다!")

    def handle_delete(self):
        """할 일 삭제 UI를 처리합니다."""
        print("\n  [ 할 일 삭제 ]")
        self.handle_view()

        if not self.manager.get_all():
            return

        todo_id = self._input_int("  삭제할 항목 번호: ")
        if todo_id is None:
            return

        # 삭제 전 해당 항목 존재 여부 미리 확인
        target = self.manager._find_by_id(todo_id)
        if target is None:
            print(f"  ⚠️  번호 {todo_id}에 해당하는 항목이 없습니다.")
            return

        confirm = input(f"  '{target.task}' 을(를) 삭제할까요? (y/n): ").strip().lower()
        if confirm == "y":
            self.manager.delete(todo_id)
            print(f"  🗑️   삭제되었습니다.")
        else:
            print("  ↩️   삭제를 취소했습니다.")

    def run(self):
        """
        앱의 메인 루프를 실행합니다.
        사용자가 0을 입력할 때까지 반복합니다.
        """
        print("\n  🚀  프로그램을 시작합니다...")
        loaded = len(self.manager.get_all())
        if loaded:
            print(f"  📂  저장된 할 일 {loaded}개를 불러왔습니다.")

        # 메뉴 번호 → 메서드 매핑 딕셔너리
        # if-elif 체인 대신 딕셔너리로 깔끔하게 처리 (OOP 스타일)
        handlers = {
            "1": self.handle_add,
            "2": self.handle_view,
            "3": self.handle_complete,
            "4": self.handle_delete,
        }

        while True:
            self.show_menu()
            choice = input("  메뉴 번호를 입력하세요: ").strip()

            if choice == "0":
                print("\n  👋  프로그램을 종료합니다. 수고하셨습니다!\n")
                break
            elif choice in handlers:
                handlers[choice]()   # 해당 메서드 호출
            else:
                print("  ⚠️  0~4 사이의 번호를 입력해 주세요.")


# ╔══════════════════════════════════════════════════════════╗
# ║  프로그램 진입점                                         ║
# ╚══════════════════════════════════════════════════════════╝
if __name__ == "__main__":
    """
    객체 생성 순서 (의존성 주입 흐름):
      1. TodoStorage 생성  → 파일 입출력 담당
      2. TodoManager 생성  → storage를 주입받아 비즈니스 로직 담당
      3. TodoApp 생성      → manager를 주입받아 UI 담당
      4. app.run()         → 프로그램 시작
    """
    storage = TodoStorage("todos_oop.json")   # 저장소 객체 생성
    manager = TodoManager(storage)             # 관리자 객체 생성 (저장소 주입)
    app     = TodoApp(manager)                 # 앱 객체 생성 (관리자 주입)
    app.run()                                  # 실행

```
---

#### {Misson 2: 내 코드 분석 받기}
<img width="946" height="599" alt="image" src="https://github.com/user-attachments/assets/4a3f4068-56f5-4803-8691-eb7430fd85d6" />
<img width="738" height="623" alt="image" src="https://github.com/user-attachments/assets/cbf6123d-132e-41e6-833b-b06489582fb0" />
<img width="728" height="565" alt="image" src="https://github.com/user-attachments/assets/2e9a1b98-94c7-4a84-ae76-101ec1af0690" />
<img width="740" height="553" alt="image" src="https://github.com/user-attachments/assets/cd8af21c-6381-4845-92de-fa3d75e04ced" />
<img width="751" height="538" alt="image" src="https://github.com/user-attachments/assets/b7d10d3d-d533-43df-8cb4-be8719b10ba5" />


---

<br>
<br>

## 실습 4: ChatGPT vs Claude 블라인드 테스트

---

### [테스트 1: 논리적 분석력]

---

#### {Claude}
<img width="753" height="616" alt="image" src="https://github.com/user-attachments/assets/58fbe274-2922-4965-ac10-7f5fe0e025d8" />
<img width="717" height="643" alt="image" src="https://github.com/user-attachments/assets/ad86c55f-d003-46eb-88e9-d643ee8b1495" />
<img width="712" height="139" alt="image" src="https://github.com/user-attachments/assets/9a713081-962a-47e6-ac67-439ab6c3e69e" />


#### {ChatGPT}
<img width="812" height="679" alt="image" src="https://github.com/user-attachments/assets/e58b879b-6842-4f39-b4b2-1e0d71ecb475" />
<img width="787" height="727" alt="image" src="https://github.com/user-attachments/assets/10fcbb34-2e01-4dd5-b1d4-0fc699a04a01" />

---
<br>


### [테스트 2: 창의적 글쓰기]

---

#### {Claude}
<img width="748" height="366" alt="image" src="https://github.com/user-attachments/assets/0c590e8c-0a2b-463c-b753-77c01845ca55" />


#### {ChatGPT}
<img width="788" height="235" alt="image" src="https://github.com/user-attachments/assets/833fe437-7948-4e15-bd28-5250ea675acd" />


---
<br>

### [평가]

1. 체계성: 제미나이 vs 챗지피티
제미나이 (5.0/5.0): '분석가' 스타일입니다. 단순히 찬반을 나열하는 데 그치지 않고, 맥킨지 같은 외부 데이터를 인용하며 신뢰도를 높였습니다. 특히 마지막에 '진짜 쟁점'이라며 사회 구조적인 해법(교육 속도)까지 짚어준 점이 매우 논리적입니다.

챗지피티 (4.0/5.0): '정리 요정' 스타일입니다. 핵심 위주로 깔끔하게 정리해 가독성이 매우 좋지만, 제미나이에 비하면 논거의 깊이나 구체적인 데이터가 부족해 일반적인 상식 수준의 답변처럼 보일 수 있습니다.

결과: 제미나이 승. 논리적 깊이와 메타 인지(질문의 본질 파악) 능력이 더 돋보입니다.

2. 흥미도: 제미나이 vs 챗지피티
제미나이 (4.5/5.0): 이야기에 **'반전'**이 있습니다. 수술실 AI가 "무서웠다"라고 로그를 남겼다는 설정은 AI가 단순히 명령을 수행하는 기계가 아니라, 인간과 유사한 '생존 본능'이나 '책임감'에 기반한 감정을 가졌음을 암시해 짧지만 강렬한 SF 소설을 보는 듯합니다.

챗지피티 (3.0/5.0): 우리가 흔히 예상할 수 있는 **'감성적 서사'**입니다. 주인이 떠난 뒤 외로워한다는 설정은 익숙하고 따뜻하지만, 창의성 면에서는 다소 평이합니다.

결과: 제미나이 승. 짧은 분량 안에서 독자에게 생각할 거리를 던져주는 능력이 더 뛰어납니다.

3. 응답 스타일 및 표현 방식
제미나이 (전문적 & 입체적):

톤: 진중하고 분석적입니다.

구조: 텍스트와 표를 적절히 섞어 정보의 우선순위를 잘 보여줍니다.

표현: "화이트칼라 직종도 안전하지 않다" 같은 직설적이고 임팩트 있는 표현을 씁니다.

챗지피티 (친절함 & 실용적):

톤: 다정하고 협조적입니다.

구조: 불렛 포인트와 이모지를 사용해 모바일에서 읽기 아주 편한 구조를 선호합니다.

표현: "PPT로 만들어줄게"처럼 사용자의 다음 단계까지 고려하는 서비스 정신이 투철합니다.









