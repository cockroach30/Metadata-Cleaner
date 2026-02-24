# Metadata-Cleaner



Metadata Cleaner Tool Prerequisites
Required Library

- pip install pillow mutagen python-docx openpyxl python-pptx


After running the tool, you choose whether you want to remove or display the metadata of the file in question.

# Metadata-Cleaner-code

    - import os
    - import platform
    - from datetime import datetime
    - from PIL import Image, ExifTags
    - from mutagen import File
    - from docx import Document
    - from openpyxl import load_workbook
    - from pptx import Presentation

# ---------------- SYSTEM META ----------------

    def system_metadata(path):
    stats = os.stat(path)
    print("\n--- System Metadata ---")
    print("Size:", stats.st_size, "bytes")
    print("Created:", datetime.fromtimestamp(stats.st_ctime))
    print("Modified:", datetime.fromtimestamp(stats.st_mtime))
    print("Accessed:", datetime.fromtimestamp(stats.st_atime))
    print("OS:", platform.system())

# ---------------- IMAGE META ----------------
    def view_image_meta(path):
    try:
        img = Image.open(path)
        exif = img.getexif()
        print("\n--- Image Metadata ---")
        if exif:
            for tag_id in exif:
                tag = ExifTags.TAGS.get(tag_id, tag_id)
                print(f"{tag}: {exif.get(tag_id)}")
        else:
            print("No EXIF metadata")
    except:
        pass

# ---------------- AUDIO META ----------------
    def view_audio_meta(path):
        try:
            audio = File(path)
            print("\n--- Audio Metadata ---")
            if audio and audio.tags:
               for key, value in audio.tags.items():
                   print(f"{key}: {value}")
           else:
                print("No audio metadata")
          except:
                 pass

# ---------------- PY META ----------------
    def view_py_meta(path):
       print("\n--- Python Metadata ---")
       with open(path, "r", encoding="utf-8") as f:
           for line in f:
               if line.startswith("#"):
                   print("Comment:", line.strip())
               if "coding" in line:
                   print("Encoding:", line.strip())
               if '"""' in line or "'''" in line:
                   print("Docstring:", line.strip())

# ---------------- TXT META ----------------
    def view_txt_meta(path):
        print("\n--- TXT Metadata ---")
        print("No internal metadata. Only system metadata exists.")

# ---------------- DOCX META ----------------
    def view_docx_meta(path):
        try:
            doc = Document(path)
            props = doc.core_properties
            print("\n--- DOCX Metadata ---")
            print("Author:", props.author)
            print("Created:", props.created)
            print("Modified:", props.modified)
            print("Last Modified By:", props.last_modified_by)
        except:
            pass

# ---------------- XLSX META ----------------
    def view_xlsx_meta(path):
        try:
            wb = load_workbook(path)
            props = wb.properties
            print("\n--- XLSX Metadata ---")
            print("Creator:", props.creator)
            print("Created:", props.created)
            print("Modified:", props.modified)
        except:
            pass

# ---------------- PPT META ----------------
    def view_pptx_meta(path):
        try:
            prs = Presentation(path)
            props = prs.core_properties
            print("\n--- PPTX Metadata ---")
            print("Author:", props.author)
            print("Created:", props.created)
            print("Modified:", props.modified)
        except:
            pass

# ---------------- CLEAN METHODS ----------------
    def clean_rebuild(input_path, output_path):
        with open(input_path, "rb") as f:
            data = f.read()
        with open(output_path, "wb") as f:
            f.write(data)

# ---------------- MAIN VIEW ----------------
    def view_metadata(path):
        ext = os.path.splitext(path)[1].lower()

    system_metadata(path)

    if ext in [".jpg", ".jpeg", ".png"]:
        view_image_meta(path)

    elif ext in [".mp3", ".wav"]:
        view_audio_meta(path)

    elif ext == ".py":
        view_py_meta(path)

    elif ext == ".txt":
        view_txt_meta(path)

    elif ext == ".docx":
        view_docx_meta(path)

    elif ext == ".xlsx":
        view_xlsx_meta(path)

    elif ext == ".pptx":
        view_pptx_meta(path)

# ---------------- MAIN CLEAN ----------------
    def clean_metadata(path):
        output = "clean_" + os.path.basename(path)
        clean_rebuild(path, output)
        print("\n✅ Clean file saved as:", output)

    # ---------------- RUN ----------------
    print("1️⃣ View Metadata")
    print("2️⃣ Clean Metadata")

    choice = input("Choose (1 or 2): ")
    file_path = input("Enter file path: ")

    if choice == "1":
        view_metadata(file_path)
    elif choice == "2":
        clean_metadata(file_path)
    else:
        print("Invalid choice")


    
