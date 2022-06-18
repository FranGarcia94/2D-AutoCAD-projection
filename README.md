# 2D-AutoCAD-projection-with-AutoLISP
Go from an AutoCAD sheet with entities with a z-value other of 0 to a two-dimensional projection by placing those elements on the z-axis value 0.

It is possible that you need to do this action for some reason as it happened to me. In my case, the number of sheets in which I had to perform this action was very large (around 600), so I decided to create this code to automate the process.

## Code description

### 1. Select all the objects in the drawing and save them in the variable 'Conjunto'
    (SETQ Conjunto (SSGET "x")); Where x implies: Entire database. If you specify the X selection method and do not provide a filter-list, ssget selects all entities in the database, including entities on layers that are off, frozen, and out of the visible screen.
### 2. Move that selection on the z axis until it is at 0
    (COMMAND "_.move" Conjunto "" "0,0,1e99" "")
    (COMMAND "_.move" Conjunto "" "0,0,-1e99" "")
### 3. Choose the folder and the name where the file will be saved
    (SETQ name (strcat "C:\\Users... INSERT PATH TO SAVE NEW FILE" "2D_" (getvar "DWGNAME"))); Change path. Here the final name will be 2D_ followed by the original name of your file. For example: 2D_sheet_1
### 4. Save the modified file in the selected folder and with the chosen name
    (command "_.SAVE" name)
### 5. Generates a list with the names of all '.dwg' files in the working folder
    (SETQ lista (vl-directory-files (getvar "dwgprefix") "*.dwg" 1))
### 6. Create a counter, initialized to 0, to know what file we are in and be able to move on to the next one
    (SETQ cont 0) ; Crea el contador
    (WHILE (/= (nth cont lista) (getvar "DWGNAME"))
    (setq cont (+ cont 1)) )
    (setq cont (+ cont 1))
    (SETQ arch (nth cont lista)); Save in the variable 'arch' the name of the file following the current.
### 7. Open the file in the current folder with the name saved in 'arch', that is to say, open the following '.dwg' file to perform the same action.
    (command "_.FILEOPEN" arch)
    
## Upload the file
1. Save the file with '.lsp' format
2. In the autocad toolbar, select the 'Manage' tab
3. Within 'Manage' select 'Load application', a pop-up window will open.
4. Go to the working folder and select the '.lsp' file and press 'load'
5. We will also see a section called 'Load at startup', click on the 'Content' button. A pop-up window will open showing the files that are running when you open a drawing. An empty list should appear.
6. Click on 'Add' and select the '.lsp' file with our code.
7. Close all the windows

### Warning
- Once we close the windows, the file will be executed in the current drawing and the next drawing will be opened and this one will be executed again. This process will end when the last '.dwg' file in the folder is executed.
- You can press ESC to stop the process at any time.
- The original drawings are not modified but they may be depending on the version of AutoCAD. It is recommended to make a copy.

## Running the code

We could have something like this:

Two folders, one with the original files and another empty where the new files will be saved.
<br></br>
![img_hojas(1)](https://user-images.githubusercontent.com/107102754/174443815-841939b1-c662-4367-a662-2b34234f5a4b.jpg)
![img_hojas_2d_1(1)](https://user-images.githubusercontent.com/107102754/174443822-8c698108-5188-472d-9e52-43888b58288d.jpg)

This is a test file where the program will run. As we can see there are elements with a certain elevation.
:---:
![img_1(1)](https://user-images.githubusercontent.com/107102754/174443941-ddf32059-d382-4030-842a-f4b188214891.jpg)
![img_2](https://user-images.githubusercontent.com/107102754/174444134-b4463c2e-2e26-49a6-a610-5a1a85887a45.jpg)

After running the program we will get the following:

The new files already modified will appear in the empty folder
:---:
![img_hojas_2d_2(1)](https://user-images.githubusercontent.com/107102754/174444043-557223d5-6cdf-4e97-8cab-8a757a9f405a.jpg)

This is what the file would look like. All elements at dimension 0:
:---:
![img_3](https://user-images.githubusercontent.com/107102754/174444084-4cfbba45-1215-4a92-a577-e60df398bbd8.jpg)

The most interesting thing is being able to do this process automatically for all the files.


