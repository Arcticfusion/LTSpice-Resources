# LTSpice Resources

These are resources that I have made to simulate components in LTSpice.

Models are based on datasheets and PSpice models. No guarantees that the models behave as expected.

This resource is intended for educational purposes.

I would have loved to have had this resource earlier in my Electrical Engineering studies. Here's to making it easier for everyone.

----

## 1. Data Sources

Resources have been created using instructions from [this tutorial](http://www.simonbramble.co.uk/lt_spice/ltspice_lt_spice_tutorial_6.html) by Simon Bramble.

All datasheets have been found publicly online. You can find the specific referenced files in `references.md`.

To get the original PSpice files, you need access to the PSpice application. You can get a trial [here](https://www.orcad.com/pspice-free-trial). It is only available for Windows machines. Technically, once you have this, you should be able to find all the components you need in the files and will likely not need my resources. `cmp.md` maps all the subcircuits found in all the `.lib` files to their respective `.lib` files. This will hopefully help you find any models you need.

----

## 2. Adding Resources to LTSpice

Models and Libraries can be inserted into a single project or they can be added to the default libraries. To add to a file, simply add a `SPICE directive` element to your circuit `.asc` file. Locate the file that contains the required resource. Then input the following, modifying the path and file name as necessary. Note, a local path originates from the directory the circuit file is located.

```SPICE
.include local/path/to/file.spi
```

OR

```SPICE
.include /absolute/path/to/file.spi
```

To add to the default library, you will need to find the application data for LTSpice.

On MacOS: `~/Library/Application Support/LTSpice/`
On Windows: `\???`

Consider this the root directory for the following instructions.

### 2.1 Adding Symbols `.asy`

These symbols are often only the visual representation of the component. If the component is not excessively complicated, all its information can be contained in this file. Otherwise, the symbol will reference a library or another SPICE model.

When adding an independent symbol from a `.asy` file, just put a copy of the file in `lib/sym/`. You can add subfolders as desired. All are accessible inside LTSpice's component picker.

### 2.2 Adding Models `.spi` and Libraries `.lib`

`.spi` and `.lib` are usually the same thing. A `.lib` file may be used for multiple models, however, since it is a library. A `.spi` file by contrast is a single SPICE model. These may have several models, but these will generally combine into a single subcircuit model using the `.subckt` command.

Per project, these files can be added with a `SPICE directive` to include the file as described above. Once the directive has been run at least once, the component will be available from the Component Menu if it has a symbol. If it does not have a symbol, you may have to create one. These can be autogenerated.

To add them to your library of components, a copy of the file can be placed in `lib/sub/` which is where subcircuits are located. Most of these are in `.lib` files. The name is not super concerning, but should not be changed after you make the symbol. See [Section 2.3](#23-creating-symbols-for-additional-models) for how to make these.

Alternatively, you can create another folder in `lib/` to house all your additional libraries. The location just must be static for your symbols.

----

A more invasive method of adding the model is to modify the standard list. This should only be used for single line models. These likely have a default library that can be modified. Simply add the line to the file.

For a contrived single line example:

```SPICE
.model EXAMPLE_MODEL CONSTRUCTOR(a=1 b=2 c=3)
```

These models can be found in `lib/cmp/` where there are several `standard.<extension>` files. Each extension is a different component type. Each component type has its own `CONSTRUCTOR()` which you can observe in the file. Any models added to these files should also use the same constructor.

If any of the default libraries have been modified, the application will need to be restarted to access these values. Once you have restarted, the model should be available as a modifier for standard components.

I would suggest keeping another copy of these files (both the additional model and the standard component files) in case the original is corrupted.

### 2.3 Creating Symbols for Additional Models

If a model is created using the subcircuit command, it likely does not have a symbol already associated with it.

1. Open the `.spi`, `.lib`, or other file with the `.subckt` command in LTSpice.
2. Locate the line that starts with `.subckt <required-component>` and right click on it.
3. Select `Create Symbol` from the drop down menu that appeared.

This will create a default symbol with all the requested pins in the `.subckt` command. If you want the symbol to look more like another component, you can look at that component's file and copy over most of the symbol.

This symbol can be edited graphically through LTSpice, or it can be edited as text in an IDE or text application of your choice. I prefer the text method as I can be sure to copy over all the visual components. This includes lines starting with `PIN`, `PINATTR`, `LINE`, `WINDOW`, `RECTANGLE`, etc. Make sure you still have the same number of pins and that they have the same expected `PINATTR SpiceOrder`.

> DO NOT write over any `SYMATTR` lines without knowing what they do. These are the simulation attributes.

----

Symbols created with this method will be located in `lib/sym/AutoGenerated/`. These files contain a reference to the file that is used to create the symbol. Make sure this file is in its final location before then.

Therefore, if you create a symbol for a local instance of a subcircuit, this will link to the local instance. This can become an issue if it ever moves location or changes name as the reference will be broken.

## 3. Other Resources

Here is a list of all the keyboard shortcuts for LTSPice.

<https://www.analog.com/media/en/news-marketing-collateral/solutions-bulletins-brochures/ltspice_shortcutflyer.pdf>
