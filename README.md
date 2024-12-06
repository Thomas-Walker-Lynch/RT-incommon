
This project holds the builds for simple tools and scripts tha are common to
multiple projects, documents that are on general topics or shared by projects,
and useful configuration files.

Currently RT-incommon does not implement the full project skeleton and work flows.
For example, instead of releasing developed work product from the developer directory,
there are symbolic links into the developer directory. Also the tester directory
is empty.

I've avoided using Mosaic or Ariadne here so as to avoid hard to think about
circular dependencies.  At some point we plan to christen a release of Mosaic
and Ariadne as the go-to versions, then incorproate those go-to versions into
later versions of themselves, and here in the RT-incommon project.

This project is used by including it as a third party tool, then picking
and choosing what is to be used in the adopting project by creating
symbolic links to the chosen items in the Rt-incommon release directory.










