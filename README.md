# odoo_dev_tools
This is a collection of terminal tools, that can help developing odoo modules.

find_method.py:

This script finds all methods with the given name in the given class in all modules/parent classes. This helps when overriding methods, to be sure that other modules do not result in conflicts. The standard IDE tools to find methods in parent classes do not work here, because Odoo has it's own inheritance syntax (_inherit / _name).

Example:
1) First locate the directory which is the first common directory for default odoo addons and external odoo addons.
 i.e. cd to opt/odoo if the installation looks as following:
    -> opt/odoo
    |--> opt/odoo/addons
    |--> opt/odoo/third_party_addons
2) run:  python find_method.py odoo.class.name method_name
3) The result will list all files, where the method_name of this class is defined
