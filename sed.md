# show lines 20 to 40,

sed -n '20,40p;41q' file_name
awk 'FNR>=20 && FNR<=40' file_name


# print line number 52
sed -n '52p' # method 1
sed '52!d' # method 2
sed '52q;d' # method 3,  efficient on large files 
