# libpqxx for Windows
This is distribution of libpqxx tailored for Microsoft Windows. It has been customised for the Microsoft Visual Studio toolchain. It has been tested with Visual Studio 2015 Community Edition.

## Build instructions
#### Step 1: Create a Project Properties sheet to hold the configuration macros
Create a new (empty) project in Visual Studio for libpqxx and import the source directory and header files from this repository. In order to get visual studio to build the library you will need to set some parameters. This is most easily achieved via the property manager. Select **View** -> **Other Windows** -> **Property Manager**. Right click on any one of the build setups and select **Add New Project Property Sheet...**. Name the new sheet **ConfigMacros**. Under the **User Macros** section define the following macros:
- `PQSQL_DIR` = [PostgreSQL system installating directory]
- `LIBPQ_DEPS` = `"libeay32.dll libiconv-2.dll libintl-8.dll"`
- `REQUIRED_LIBS` = `kernel32.lib; ws2_32.lib; advapi32.lib; shell32.lib; secur32.lib; wldap32.lib;`
- `CC_FLAGS` = `PQXX_INTERNAL`
- `INCLUDE_DIR` = [Absolute path to include file of this project

Please note that the LIBPQ_DEPS macros value is dependent on your version of libpq. libpq version 9.5 requires Please note that the LIBPQ_DEPS macros value is dependent on your version of libpq. libpq version 9.5 requires libeay32.dll, libiconv-2.dll and libintl-8.dll but other versions may have different requirements - so check!

#### Step 2: Associate your project property sheet with all of your build scenarios
Now for each of the remaining build scenarios listed in the property manager right click and select **Add Existing Property Sheet...** and then select the **ConfigMacros** property sheet you have just created. Once you have done this all of the build scenarios should be using the same **ConfigMacros** property sheet.

#### Step 3: Set your project properties using the macros you have defined
Finally, we need to set the project properties. Make the following changes by right clicking on the project in the **Solution Explorer** and then selecting **Properties** (Ensure that the **Configuration** and **Platform** options are set to **All Configurations** and **All Platforms**):
- **C++** -> **General** -> **Additional Include Directories** = `$(INCLUDE_DIR);$(PGSQL_DIR)\include;%(AdditionalIncludeDirectories)`
- **C++** -> **Preprocessor** -> **Preprocesor Definitions** = `$(CC_FLAGS);%(PreprocessorDefinitions)`
- **Linker** -> **General** -> **Additional Library Directories** = `$(PGSQL_DIR)\lib;%(AdditionalLibraryDirectories)`
- **Linker** -> **Input** -> **Additional Dependencies** = `libpq.lib;$(REQUIRED_LIBS)`
- **Build Events** -> **Post-build Events** -> **Command Line** = `robocopy "$(PGSQL_DIR)\bin" "$(OUTDIR)."  libpq.dll $(LIBPQ_DEPS)`

#### Step 4: Build
The binary package should now build successfully.