# Module Import

Namespace all imported modules rather than polluting the global namespace.

This is an investigation into different ways of loading modules inspired by the Node.js Module loading system.

When a file is loaded, the result is encapsulated inside a module, and the desired module/class is exported as `EXPORTS` and returned from the function.



### Benefits

Better encapsulation, there are reduced side effects from importing modules. 

You can load two different versions of the same module without having them clash in the global namespace.

### Drawbacks

It's slower, we're using eval which is pretty slow.

Doesn't work with existing libraries. it's not a replacement for require but can be used along side it.

Likely won't work with C extensions.

## Example

Given a module in file `line.rb`, where the `Line` module is set as `EXPORTS`:

```ruby
module Line
  def Line.check
    puts 'Line check'
  end
end

EXPORTS = Line
```
    
This can be imported using `ModuleImport.import`:

```ruby
LineModule = ModuleImport.import('./other_line.rb')
    
LineModule.check
```

## Alternatives

Unsurprisingly, I'm not the first person to have this idea, @soveran had a similar idea a few years ago: https://github.com/soveran/cargo/

The implementation is a bit cleaner, it uses Kernel.load which loads the file completely seperately and returns only the exported module.

## TODO

* Load paths in a similar way to require, searching load paths etc
* Caching of loaded modules
* Tests
* More Examples
* Publish as a rubygem
* Investigate importing on a thread to force seperation (using Marshal for passing `EXPORTS` back)

## Development

Source hosted at [GitHub](http://github.com/andrew/module_import).
Report Issues/Feature requests on [GitHub Issues](http://github.com/andrew/module_import/issues).

Tests can be ran with `rake spec`

### Note on Patches/Pull Requests

 * Fork the project.
 * Make your feature addition or bug fix.
 * Add tests for it. This is important so I don't break it in a
   future version unintentionally.
 * Add documentation if necessary.
 * Commit, do not mess with rakefile, version, or history.
   (if you want to have your own version, that is fine but bump version in a commit by itself I can ignore when I pull)
 * Send a pull request. Bonus points for topic branches.

## Copyright

Copyright (c) 2013 Andrew Nesbitt. See [LICENSE](https://github.com/andrew/module_import/blob/master/LICENSE) for details.
