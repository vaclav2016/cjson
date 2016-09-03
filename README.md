# Very low footprint JSON parser/builder written in portable ANSI C.

This a fork of original repository with my small changes: cmake-integration and some fixes. Reason: I don't want have json.c and json.h at each my project, but `sudo make install` has been broken in both original repository. Also, only one of them support cmake. So, I create fork repository with more friendly installing process.

* BSD licensed with no dependencies (i.e. just drop the C file into your project)
* Never recurses or allocates more memory than it needs
* Very simple API with operator sugar for C++

Installing
----------

    $ cmake ./
    $ make
    $ sudo make install

API
---

    json_value * json_parse (const json_char * json,
                             size_t length);

    json_value * json_parse_ex (json_settings * settings,
                                const json_char * json,
                                size_t length,
                                char * error);

    void json_value_free (json_value *);

The `type` field of `json_value` is one of:

* `json_object` (see `u.object.length`, `u.object.values[x].name`, `u.object.values[x].value`)
* `json_array` (see `u.array.length`, `u.array.values`)
* `json_integer` (see `u.integer`)
* `json_double` (see `u.dbl`)
* `json_string` (see `u.string.ptr`, `u.string.length`)
* `json_boolean` (see `u.boolean`)
* `json_null`


Compile-Time Options
--------------------

    -DJSON_TRACK_SOURCE

Stores the source location (line and column number) inside each `json_value`.

This is useful for application-level error reporting.


Runtime Options
---------------

    settings |= json_enable_comments;

Enables C-style `// line` and `/* block */` comments.

    size_t value_extra

The amount of space (if any) to allocate at the end of each `json_value`, in
order to give the application space to add metadata.

    void * (* mem_alloc) (size_t, int zero, void * user_data);
    void (* mem_free) (void *, void * user_data);

Custom allocator routines.  If NULL, the default `malloc` and `free` will be used.

The `user_data` pointer will be forwarded from `json_settings` to allow application
context to be passed.


