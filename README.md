# R36S-U-Boot-PanelSupport
u-boot dtb for each panel, organized, and a panel chooser to be used with [R36s-U-Boot](https://github.com/R36S-Stuff/R36S-u-boot): [Download here](https://github.com/R36S-Stuff/R36S-u-boot-builder/releases/tag/v1)


## Uboot loads env AFTER board setup, meaning the logo and logo dtb path was hardcoded

### so before dtb gets loaded, i've added this

```
if load mmc 1:1 ${loadaddr} logo.env; then;
    env import -t ${loadaddr} ${filesize};
fi
```
Example `logo.env` file contents: (note the trailing slash)

```PanelPathSlash=ScreenFiles/Panel 4/ ```

I've also added a variable before the dtb name, without a slash.

if the var is not populated, and logo.env is not present, uboot behaves as expected.

However, if PanelPathSlash is populated, uboot will load from that path. allowing for panel dtb for the logo to be set in uboot

```
setenv dtb_name \"${PanelPathSlash}rg351mp-kernel.dtb\"", 0);
```

Then, bootcmd will try to load PanCho.ini before boot.ini

```
####################################################################################
########### ALL OTHER ENV SHOULD BE PERSISTED USING THE saveenv COMMAND ############
### FAT OVERWRITES ARE UNSTABLE IN UBOOT THIS DOES THE LEAST AMOUNT OF FATWRITES ###
####################################################################################
```

# Hold R1 to set panel, Set panel via buttons
```
U         =   New Panel 1
R         =   New Panel 2
D         =   New Panel 3
L         =   New Panel 4
X(north)  =   Empty
A(east)   =   Original Panel
B(south)  =   RGB20S
Y(west)   =   Custom
```




```
# # # # # # # # # # # # # # # # # # # #
#  _________________________________  #
# |                                 | #
# |                                 | #
# |                                 | #
# |                                 | #
# |                                 | #
# |                                 | #
# |                                 | #
# |                                 | #
# |_________________________________| #
#                                     #
#         1                           #
#         ▲               (X)         #
#     4 ◄   ► 2   Custom(Y) (A)Orig   #
#         ▼               (B)         #
#         3              RGB20S       #
#         _                _          #
#       /   \            /   \        #
#      |     |          |     |       #
#       \ _ /            \ _ /        #
#                                     #
# # # # # # # # # # # # # # # # # # # #
```



> (included logo.bmp is from [sect2k](https://github.com/sect2k/r36s-outline))

> thanks to @rocknix for the idea/inspiration and the gpio reading example from their `boot.ini`