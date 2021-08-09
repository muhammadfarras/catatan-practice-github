## GIT ALIAS
Gi Alias adalah sebuah cara untuk mengotomasi atau memperpendek sebuah perintah. Menggunakan git alias membuat pengembangan sebuah aplikasi menjadi lebih efesien. 
Sebagai contoh 
```
$ git config --global alias.st status
```
Dari pada kita selalau mengetik `git status` kita dapat memperpendek perintahnya menjadi `git st`, dan perintah tersebut akan menampilkan hal yang sama dengan `git status`.

Git Alias untuk memigrasi sebuah *commits* dari satu branch ke branch yang lainya sangat kompleks. Agar lebih efesien kita dapat membuat git alias untuk migrasi dari pada kita mengetik beberapa command untuk hasil yang sama.

## Langkah Membuat Git Alias (Migrate)
Ikut beberap langkah dibawah ini.
1. buka `.gitconfig` pada folder `.git`
2. Tambahkan kode berikut pada baris paling bawah dalam file `.gitconfig`
```
[alias]
migrate = "!f(){ CURRENT=$(git symbolic-ref --short HEAD); 
git checkout -b $1 && git branch --force $CURRENT 
${3-$CURRENT@{u}} && git rebase --onto ${2-master} $CURRENT; 
}; f"
```
> Jika didalam file .gitconfig kalian sudah memliki kelompok [alias], kalian tidak perlu membuat [alias] baru.

3. Setelah itu kalian dapat menggunakan perintah git migrete untuk migrasi commits dari satu branch ke branch yang lainnya. 

Git ini memerlukan satu parameter wajib dan dua parameter opsional.

```
git migrate <new-branch-name> <target-branch> <commit-range>
```

* Parameter `<new-branch-name>` wajib diisi. Branch ini adalah tempat kita ingin memindahkan commits kita, jika kita tidak mengisi kecualian parameter ini saja maka perintah migrasi akan memindahkan semua commits kedalam master branch ke branch ini (nama branch pada parameter ini).

* `<target-branch>` opsional. `<targetbranch>` menginjinkan kita dapat memindahkan commits dari sebuah branch (selain branch master) ke `<new-branch-name>` (nama branch pada parameter pertama).

* `<commit-range>` opsional. Mengijikan kita untuk dapat menspesifikasi commits yang mana yang ingin dipindahkan. Parameter ini akan sangat berguna jika secara tidak sengaja kita membuat sebuah commits pada branch yang salah, dan kita ingin memindahkan commits tersebut ke `<new-branch-name>`.
