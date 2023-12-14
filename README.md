import firebase_admin

from firebase_admin import credentials, db

cred = credentials.Certificate(".gitignore/key.json")

firebase_admin.initialize_app(
    cred, {"databaseURL": "https://notes-database-182af-default-rtdb.firebaseio.com/"}
)

# reference
ref = db.reference("/")


# get note details
def create_note():
    title = input("Note Title: ")
    content = input("Note Content: ")
    note_data = {
        "title": title,
        "content": content,
    }
    return note_data


# add a node called notes and add an object
def add_note(note_data):
    new_note_ref = ref.child("notes").push(note_data)


# read the data
def get_notes():
    return ref.child("notes").get()


# print notes
def print_notes(snapshot):
    if snapshot:
        print("Notes")
        for key, value in snapshot.items():
            print(f"{key}: {value}")
    else:
        print("No notes available, create a new one!")


# delete the notes data
def delete_notes():
    ref.child("notes").delete()


# main loop

x = 1
while True:
    x = int(input("1: Create Note \n2: Read Notes \n3: Delete Notes \n0: Quit \n"))
    if x == 0:
        break
    elif x == 1:
        note = create_note()
        add_note(note)
    elif x == 2:
        notes = get_notes()
        print_notes(notes)
    elif x == 3:
        delete_notes()
    else:
        print("Oops, try again.")
