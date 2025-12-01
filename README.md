# Periodic Table using Python Tkinter

This is my 12th class project, where I have created a GUI-based Periodic Table where one can view images and details of elements.

## Outcome of the project
* Shows the position of the elements in the periodic table
* Display the element's information and image
* Tell random facts about the elements of the periodic table

## Setup and Installation

You can install Python 3.11 or a higher version on your desktop.
<br>
For the installation of the module, you can either install it in your command prompt or run the following code
<br>
```bash
  import subprocess
  import sys
  def import_or_install(module_name):
      try:
          __import__(module_name)
          pass
      except ImportError:
          print(f"The module '{module_name}' is not installed. Installing now...")
          subprocess.check_call([sys.executable, "-m", "pip", "install", module_name])
          pr int(f"'{module_name}' has been installed successfully.")
  import_or_install('tkinter')
```
<br>
This is used when we have to share the exact code on someone else's laptop and send the file as it is. This block of code checks whether the modules are pre-installed, and if not, then installs them before continuing further.

## Creation of Database
The code provides logic, where the program will check whether the database is installed in the laptop or not. If not, it then creates the database, using python mysql.connector and stores all the information and url links in it

```bash
  cr.execute("SHOW DATABASES LIKE 'periodic_table'")
  result = cr.fetchone()
  if not result:
      cr.execute('create database periodic_table')
      cr.execute('use periodic_table')
  ...
```

## Loading Image URL

Retrieve the image URL using Pillow, io and requests module
1. Pillow (PIL) module is used for image processing. It opens, loads and saves the images
2. The io module enables working with data streams held in memory, such as StringIO for text data and BytesIO for binary data.
3. requests modules allow sending different types of HTTP requests, such as GET, POST, PUT, DELETE, PATCH, and HEAD, which are essential for interacting with web servers and APIs.

```bash
    global image_label
    cr.execute(f'SELECT * FROM info WHERE atm_no = {atm_no}')
    result = cr.fetchone()
    if result:
        info = f"{result[0]}\nAtomic Number: {result[1]}\nAtomic Mass: {result[2]}\nBlock: {result[3]}\nGroup: {result[4]}\nConfiguration: {result[5]}\nApplication: {result[6]}"
        image_url = result[7] 
        
        if image_url:
            try:
                response = requests.get(image_url)
                response.raise_for_status()
                image_data = response.content
                img = Image.open(io.BytesIO(image_data))
                text_box_width = text_box.winfo_width()
                text_box_height = text_box.winfo_height()

                img_resized = img.resize((text_box_width, text_box_height), Image.LANCZOS)
                photo = ImageTk.PhotoImage(img_resized)
                
                if 'image_label' in globals():
                    image_label.destroy()  
                
                image_label = tk.Label(image_frame, image=photo)
                image_label.image = photo  
                image_label.pack(side=tk.RIGHT, fill=tk.BOTH, expand=True)  
            except requests.exceptions.RequestException as e:
                print(f"Error fetching image: {e}")
```
## Import points to Remember
* This project requires a good internet connection as it retrieves the image's URL from Google.com.
* This project may take time to load and display the image and information.
* If any changes are made in the database, ensure that the previous version of your database is removed from your laptop.

## Find a bug?
If you found an issue or would like to submit an improvement to this project, please submit an issue using the issues tab above.
