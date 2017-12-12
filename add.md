## 名称 {#_name}

git-add - 添加文件内容到索引

## 摘要 {#_synopsis}

```
git add
 [--verbose | -v] [--dry-run | -n] [--force | -f] [--interactive | -i] [--patch | -p]
      [--edit | -e] [--[no-]all | --[no-]ignore-removal | [--update | -u]]
      [--intent-to-add | -N] [--refresh] [--ignore-errors] [--ignore-missing]
      [--chmod=(+|-)x] [--] [
<
pathspec
>
…​]
```

## 描述 {#_description}

此命令用工作目录中的当前内容更新索引，为下一次提交准备暂存区中的内容。它通常情况下添加已存在路径下的所有内容，但通过选项设定也可以用于添加对于工作目录中的部分修改，或者删除已经不再存在于工作目录中的路径。

"索引"保存了工作目录内容的一份快照，在下一次提交时将提交快照中的内容。因此在对工作目录做了任何修改后，执行提交命令commit之前，你必须用add命令把新增的或修改的文件添加到索引。

此命令可以在`commit`之前执行多次。它只把`add`命令执行时指定的文件修改内容添加到索引；如果你想在下一次`commit`时把后续修改内容也包含进去，你必须再次执行`git add` 来把新内容加入索引。

`git status` 命令可以用于获取下一次提交前哪些文件被修改的总结。

`git add`命令缺省不添加被忽略的文件。如果在命令行明确指定添加某些被忽略的文件，`git add`命令会失败，并列出所有被忽略的文件。通过目录递归方式到达的忽略文件或通过Git globbing方式\(在shell命令中引用globs\)到达的忽略文件，将被静默忽略。`git add`命令可以通过增加`-f`\(强制\)选项来添加被忽略文件。

请参阅[git-commit\[1\]](https://git-scm.com/docs/git-commit)命令了解其他添加内容到提交的方法。

## 选项 {#_options}

&lt;

pathspec

&gt;

…​

Files to add content from. Fileglobs \(e.g.`*.c`\) can be given to add all matching files. Also a leading directory name \(e.g.`dir`to add`dir/file1`and`dir/file2`\) can be given to update the index to match the current state of the directory as a whole \(e.g. specifying`dir`will record not just a file`dir/file1`modified in the working tree, a file`dir/file2`added to the working tree, but also a file`dir/file3`removed from the working tree. Note that older versions of Git used to ignore removed files; use`--no-all`option if you want to add modified or new files but ignore removed ones.

For more details about the &lt;pathspec&gt; syntax, see the\_pathspec\_entry in[gitglossary\[7\]](https://git-scm.com/docs/gitglossary).

-n

--dry-run

不真正添加文件，只用于显示它们是否存在或是否将被忽略。

-v

--verbose

详细输出。

-f

--force

允许添加被忽略的文件。

-i

--interactive

Add modified contents in the working tree interactively to the index. Optional path arguments may be supplied to limit operation to a subset of the working tree. See “Interactive mode” for details.

-p

--patch

Interactively choose hunks of patch between the index and the work tree and add them to the index. This gives the user a chance to review the difference before adding modified contents to the index.

This effectively runs`add --interactive`, but bypasses the initial command menu and directly jumps to the`patch`subcommand. See “Interactive mode” for details.

-e

--edit

Open the diff vs. the index in an editor and let the user edit it. After the editor was closed, adjust the hunk headers and apply the patch to the index.

The intent of this option is to pick and choose lines of the patch to apply, or even to modify the contents of lines to be staged. This can be quicker and more flexible than using the interactive hunk selector. However, it is easy to confuse oneself and create a patch that does not apply to the index. See EDITING PATCHES below.

-u

--update

Update the index just where it already has an entry matching &lt;pathspec&gt;. This removes as well as modifies index entries to match the working tree, but adds no new files.

If no &lt;pathspec&gt; is given when`-u`option is used, all tracked files in the entire working tree are updated \(old versions of Git used to limit the update to the current directory and its subdirectories\).

-A

--all

--no-ignore-removal

Update the index not only where the working tree has a file matching &lt;pathspec&gt; but also where the index already has an entry. This adds, modifies, and removes index entries to match the working tree.

If no &lt;pathspec&gt; is given when`-A`option is used, all files in the entire working tree are updated \(old versions of Git used to limit the update to the current directory and its subdirectories\).

--no-all

--ignore-removal

Update the index by adding new files that are unknown to the index and files modified in the working tree, but ignore files that have been removed from the working tree. This option is a no-op when no &lt;pathspec&gt; is used.

This option is primarily to help users who are used to older versions of Git, whose "git add &lt;pathspec&gt;…​" was a synonym for "git add --no-all &lt;pathspec&gt;…​", i.e. ignored removed files.

-N

--intent-to-add

Record only the fact that the path will be added later. An entry for the path is placed in the index with no content. This is useful for, among other things, showing the unstaged content of such files with`git diff`and committing them with`git commit -a`.

--refresh

Don’t add the file\(s\), but only refresh their stat\(\) information in the index.

--ignore-errors

If some files could not be added because of errors indexing them, do not abort the operation, but continue adding the others. The command shall still exit with non-zero status. The configuration variable`add.ignoreErrors`can be set to true to make this the default behaviour.

--ignore-missing

This option can only be used together with --dry-run. By using this option the user can check if any of the given files would be ignored, no matter if they are already present in the work tree or not.

--no-warn-embedded-repo

By default,`git add`will warn when adding an embedded repository to the index without using`git submodule add`to create an entry in`.gitmodules`. This option will suppress the warning \(e.g., if you are manually performing operations on submodules\).

--chmod=\(+\|-\)x

Override the executable bit of the added files. The executable bit is only changed in the index, the files on disk are left unchanged.

--

This option can be used to separate command-line options from the list of files, \(useful when filenames might be mistaken for command-line options\).

## 配置 {#_configuration}

The optional configuration variable`core.excludesFile`indicates a path to a file containing patterns of file names to exclude from git-add, similar to $GIT\_DIR/info/exclude. Patterns in the exclude file are used in addition to those in info/exclude. See[gitignore\[5\]](https://git-scm.com/docs/gitignore).

## 示例 {#_examples}

* Adds content from all`*.txt`files under`Documentation`directory and its subdirectories:

  ```
  $ git add Documentation/\*.txt
  ```

  Note that the asterisk`*`is quoted from the shell in this example; this lets the command include the files from subdirectories of`Documentation/`directory.

* Considers adding content from all git-\*.sh scripts:

  ```
  $ git add git-*.sh
  ```

  Because this example lets the shell expand the asterisk \(i.e. you are listing the files explicitly\), it does not consider`subdir/git-foo.sh`.

## 交互模式 {#_interactive_mode}

When the command enters the interactive mode, it shows the output of the\_status\_subcommand, and then goes into its interactive command loop.

The command loop shows the list of subcommands available, and gives a prompt "What now&gt; ". In general, when the prompt ends with a single_&gt;_, you can pick only one of the choices given and type return, like this:

```
    *** Commands ***
      1: status       2: update       3: revert       4: add untracked
      5: patch        6: diff         7: quit         8: help
    What now
>
 1
```

You also could say`s`or`sta`or`status`above as long as the choice is unique.

The main command loop has 6 subcommands \(plus help and quit\).

status

This shows the change between HEAD and index \(i.e. what will be committed if you say`git commit`\), and between index and working tree files \(i.e. what you could stage further before`git commit`using`git add`\) for each path. A sample output looks like this:

```
              staged     unstaged path
     1:       binary      nothing foo.png
     2:     +403/-35        +1/-1 git-add--interactive.perl
```

It shows that foo.png has differences from HEAD \(but that is binary so line count cannot be shown\) and there is no difference between indexed copy and the working tree version \(if the working tree version were also different,_binary\_would have been shown in place of\_nothing_\). The other file, git-add{litdd}interactive.perl, has 403 lines added and 35 lines deleted if you commit what is in the index, but working tree file has further modifications \(one addition and one deletion\).

update

This shows the status information and issues an "Update&gt;&gt;" prompt. When the prompt ends with double_&gt;&gt;_, you can make more than one selection, concatenated with whitespace or comma. Also you can say ranges. E.g. "2-5 7,9" to choose 2,3,4,5,7,9 from the list. If the second number in a range is omitted, all remaining patches are taken. E.g. "7-" to choose 7,8,9 from the list. You can say\_\*\_to choose everything.

What you chose are then highlighted with_\*_, like this:

```
           staged     unstaged path
  1:       binary      nothing foo.png
* 2:     +403/-35        +1/-1 git-add--interactive.perl
```

To remove selection, prefix the input with`-`like this:

```
Update
>
>
 -2
```

After making the selection, answer with an empty line to stage the contents of working tree files for selected paths in the index.

revert

This has a very similar UI to_update_, and the staged information for selected paths are reverted to that of the HEAD version. Reverting new paths makes them untracked.

add untracked

This has a very similar UI to_update\_and\_revert_, and lets you add untracked paths to the index.

patch

This lets you choose one path out of a\_status\_like selection. After choosing the path, it presents the diff between the index and the working tree file and asks you if you want to stage the change of each hunk. You can select one of the following options and type return:

```
y - stage this hunk
n - do not stage this hunk
q - quit; do not stage this hunk or any of the remaining ones
a - stage this hunk and all later hunks in the file
d - do not stage this hunk or any of the later hunks in the file
g - select a hunk to go to
/ - search for a hunk matching the given regex
j - leave this hunk undecided, see next undecided hunk
J - leave this hunk undecided, see next hunk
k - leave this hunk undecided, see previous undecided hunk
K - leave this hunk undecided, see previous hunk
s - split the current hunk into smaller hunks
e - manually edit the current hunk
? - print help
```

After deciding the fate for all hunks, if there is any hunk that was chosen, the index is updated with the selected hunks.

You can omit having to type return here, by setting the configuration variable`interactive.singleKey`to`true`.

diff

This lets you review what will be committed \(i.e. between HEAD and index\).

## EDITING PATCHES {#_editing_patches}

Invoking`git add -e`or selecting`e`from the interactive hunk selector will open a patch in your editor; after the editor exits, the result is applied to the index. You are free to make arbitrary changes to the patch, but note that some changes may have confusing results, or even result in a patch that cannot be applied. If you want to abort the operation entirely \(i.e., stage nothing new in the index\), simply delete all lines of the patch. The list below describes some common things you may see in a patch, and which editing operations make sense on them.

added content

Added content is represented by lines beginning with "+". You can prevent staging any addition lines by deleting them.

removed content

Removed content is represented by lines beginning with "-". You can prevent staging their removal by converting the "-" to a " " \(space\).

modified content

Modified content is represented by "-" lines \(removing the old content\) followed by "+" lines \(adding the replacement content\). You can prevent staging the modification by converting "-" lines to " ", and removing "+" lines. Beware that modifying only half of the pair is likely to introduce confusing changes to the index.

There are also more complex operations that can be performed. But beware that because the patch is applied only to the index and not the working tree, the working tree will appear to "undo" the change in the index. For example, introducing a new line into the index that is in neither the HEAD nor the working tree will stage the new line for commit, but the line will appear to be reverted in the working tree.

Avoid using these constructs, or do so with extreme caution.

removing untouched content

Content which does not differ between the index and working tree may be shown on context lines, beginning with a " " \(space\). You can stage context lines for removal by converting the space to a "-". The resulting working tree file will appear to re-add the content.

modifying existing content

One can also modify context lines by staging them for removal \(by converting " " to "-"\) and adding a "+" line with the new content. Similarly, one can modify "+" lines for existing additions or modifications. In all cases, the new modification will appear reverted in the working tree.

new content

You may also add new content that does not exist in the patch; simply add new lines, each starting with "+". The addition will appear reverted in the working tree.

There are also several operations which should be avoided entirely, as they will make the patch impossible to apply:

* adding context \(" "\) or removal \("-"\) lines

* deleting context or removal lines

* modifying the contents of context or removal lines

## 参见 {#_see_also}

[git-status\[1\]](https://git-scm.com/docs/git-status)[git-rm\[1\]](https://git-scm.com/docs/git-rm)[git-reset\[1\]](https://git-scm.com/docs/git-reset)[git-mv\[1\]](https://git-scm.com/docs/git-mv)[git-commit\[1\]](https://git-scm.com/docs/git-commit)[git-update-index\[1\]](https://git-scm.com/docs/git-update-index)

## GIT {#_git}

Part of the[git\[1\]](https://git-scm.com/docs/git)suite

