# Thor

Thor 和 rake 类似，提供了功能强大的命令行接口。

因为 Rails 的 generator 实际上是封装了 Thor ，所以还有 [Thor Actions](http://rdoc.info/github/erikhuda/thor/master/Thor/Actions.html)

## Actions

`action(instance)`  
Wraps an action object and call it accordingly to the thor class behavior.

`append_to_file(path, *args, &block) (also: #append_file)`  
Append text to a file.

`apply(path, config = {})`  
Loads an external file and execute it in the instance binding.

`chmod(path, mode, config = {})`  
Changes the mode of the given file or directory.

`comment_lines(path, flag, *args)`  
Comment all lines matching a given regex.

`copy_file(source, *args, &block)`  
Examples.

`create_file(destination, *args, &block) (also: #add_file)`  
Create a new file relative to the destination root with the given data, which is the return value of a block or a data string.

`create_link(destination, *args, &block) (also: #add_link)`  
Create a new file relative to the destination root from the given source.

`destination_root`  
Returns the root for this thor class (also aliased as destination root).

`destination_root=(root)`  
Sets the root for this thor class.

`directory(source, *args, &block)`  
Copies recursively the files from source directory to root directory.

`empty_directory(destination, config = {})`  
Creates an empty directory.

`find_in_source_paths(file)`  
Receives a file or directory and search for it in the source paths.

`get(source, *args, &block)`  
Gets the content at the given address and places it at the given relative destination.

`gsub_file(path, flag, *args, &block)`  
Run a regular expression replacement on a file.

`in_root`  
Goes to the root and execute the given block.

`initialize(args = [], options = {}, config = {})`  
Extends initializer to add more configuration options.

`inject_into_class(path, klass, *args, &block)`  
Injects text right after the class definition.

`insert_into_file(destination, *args, &block) (also: #inject_into_file)`  
Injects the given content into a file.

`inside(dir = '', config = {}, &block)`  
Do something in the root or on a provided subfolder.

`link_file(source, *args, &block)`  
Links the file from the relative source to the relative destination.

`prepend_to_file(path, *args, &block) (also: #prepend_file)`  
Prepend text to a file.

`relative_to_original_destination_root(path, remove_dot = true)`  
Returns the given path relative to the absolute root (ie, root where the script started).

`remove_file(path, config = {}) (also: #remove_dir)`  
Removes a file at the given location.

`run(command, config = {})`  
Executes a command returning the contents of the command.

`run_ruby_script(command, config = {})`  
Executes a ruby script (taking into account WIN32 platform quirks).

`source_paths`  
Holds source paths in instance so they can be manipulated.

`template(source, *args, &block)`  
Gets an ERB template at the relative source, executes it and makes a copy at the relative destination.

`thor(command, *args)`  
Run a thor command.

`uncomment_lines(path, flag, *args)`  
Uncomment all lines matching a given regex.

## Actions ClassMethods

`add_runtime_options!`  
Add runtime options that help actions execution.

`source_paths`  
Hold source paths for one Thor instance.

`source_paths_for_search`  
Returns the source paths in the following order:.

`source_root(path = nil)`  
Stores and return the source root for this class.

## 其它

`argument`  
Adds an argument to the class and creates an attr_accessor for it.

链接 [What Is Thor](http://whatisthor.com/)
