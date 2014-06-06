This is the the tree to insert in the splashscreen a page with the board of a architech partner.

partners                                        - folder in ~/architech_sdk/
    |
    |-- partner1                                - folder of partner1
    |-- partner2
    |       |-- splashscreen
    |       |       |-- logo.txt                - name of the jpg file (e.g. partner2.jpg)
    |       |       |-- long_description.txt    - not used, but needed [html tags supported]
    |       |       |-- name.txt                - name of the partner (e.g. partner2)
    |       |       |-- partner2.jpg            - jpg file, logo of the partner
    |       |       |-- short_description.txt   - short description of the partner [html tags supported]
    |       |       |-- index.html (optional)   - alternative display of the partner page in the splashscreen
    |       |-- board1
    |       |       |-- splashscreen
    |       |                   |-- board_image.txt         - board image name (e.g. board1.jpg)
    |       |                   |-- board1.jpg              - board image (e.g. board1.jpg)
    |       |                   |-- long_description.txt    - long description, [html tags supported]
    |       |                   |-- name.txt                - name of the board (e.g. artich)
    |       |                   |-- run_bitbake             - script to launch bitbake
    |       |                   |-- run_cross-compiler      - script to launch crosscompiler ambient (user space)
    |       |                   |-- run_documentation       - script to view documentation
    |       |                   |-- run_eclipse             - script to launch eclipse
    |       |                   |-- run_hob                 - script to launch hob
    |       |                   |-- run_images-folder       - script to open images built
    |       |                   |-- run_install             - script to update the board sdk
    |       |                   |-- run_qt-creator          - script to launch qt creator
    |       |                   |-- short_description.txt   - short description of the board [html tags supported]
    |       |-- board2
    |       |-- ...
    |       |-- boardN
    |-- ...
    |-- partnerN


- partner2 is the partner alias, and is choosen by architech
- index.html: the html uses the java script functions starting with "top." keyword. These functions are execute by splashscreen binary. In splashscreen_interface/index.html there are the functions used with a brief comment how-do.


In https://github.com/architech-boards/architech-manifest.git branch dora there is the manifest file "partners" where is possible add the partner boards.
The syntax is:
[partner alias] | [git repository with splashscreen directory of the partner] | [version of yocto] | [git repository with manifest of the boards partner] | [version of yocto]

- The partner alias accept the following syntax rule: [a-zA-Z][a-zA-Z_0-9]*
- git repository with splashscreen directory of the partner is the folder with the files: logo.txt, long_description.txt, name.txt, partner.jpg,...
- The current version of yocto is dora
- git repository with manifest of the boards partner is the metafile with listed all the board of the partner
- The current version of yocto is dora

In every line of the manifest file of the board is showed where download the repository of the splashscreen of the board. The syntax is:

[board alias] | [git repository with splashscreen directory of the board] | [version of yocto]

- The partner alias accept the following syntax rule: [a-zA-Z][a-zA-Z_0-9]*
- git repository with splashscreen directory of the board is the folder with the files: board_image.txt, board.jpg, long_description.txt,...
- The current version of yocto is dora
