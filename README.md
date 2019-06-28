# Property Advanced Validation Package

## Description

* Add configurable rules to check availabilty or specific expected values of properties of an Item by life cycle state at run time.

* The checks according to rules run while an item is in a defined "stage” or gets promoted to a "stage”.

* This allows to extend the options of checking for "required" data beyond just during the initial "save" action.

* "Stages" are defined as one or more possible "Life Cycle States" of an item.

* A generic "Server Method" (pev_ItemProperty_extValidations) is provided to read the rules of the ItemType specific configurations and execute the checks and generate an error message listing missing or non-conforming property values.

* This method must be connected to the "onAfterUpdate" events of ItemTypes to enable its advanced property validations. Note: on error of server methods "onAfterXX" a roll-back takes place anyways !!!

* To enable checking during a promotion to one of the defined stages, the same method must also be connected to ItemType server events "onBeforePromote" and "onAfterPromote"

  * Note: The OnBeforePromote and OnAfterPromote events can be used to define a standard promote operation for an item regardless of the lifecycle transition selected by the user. Both events are triggered BEFORE the standard lifecycle pre/post events are invoked.

* Also, an "Item Action" (pev_ItemProperty_extValidations) is provided to execute the validations upon user request to get a report of missing or non-conforming property values.

* Error messages are template based and support multi-language configurations

* Property Validation rules must be set to status "Active" (life cycle) to take effect.

## History

| Version    | Comments                                                     |
| ---------- | ------------------------------------------------------------ |
| [v1.0.0]() | Initial version. (Replaces community project "[Life Cycle Based Props and Fields](<https://github.com/ArasLabs/lc-based-props-and-fields>)") |

#### Supported Aras Versions

| Project    | Aras            |
| ---------- | --------------- |
| [v1.0.0]() | 11.0 SP14, SP15 |

## Installation

### Installation Components

* Aras Import Package Tool
* Downloaded Package
* No code tree changes

### Pre-requisites

* Aras Innovator Install (Supported Service Packs listed above)

### Dependencies

* None

### Aras Install Steps

##### Important: **Always back up your code tree and database before applying an import package or code tree patch!**

- [ ] Use the package import utility in your CD image to import the packages in this module. 

- [ ] Log on as **root** and use options <u>Merge</u> and <u>Thorough</u> for all steps
  - [ ] Import the `imports.mf` file from the Imports folder.

## Usage

1. Log in as <u>admin</u> and expand to **Administration > Configuration**
2. Open **ItemType Stages Definition**. Create a <u>New Item</u>.
3. Select the existing ItemType you wish to apply the rules to.
4. For Each Definition:
   1. Select the Itemtype you wish to pull the LC states from. 
   2. Map existing Lifecycle states to your custom stage names (Multi-lingual capable). This is a Many to One mapping.
   3. (Optional) Previous stages are used if you wish to define what the incoming state should be for the check. [For instance, if you have two branches coming into a state and you only want the validation to run if it came from Branch B and not Branch A.]
5. Save/Unlock/Close the record.
6. Open  **Property Validation Rules**. Create a <u>New Item</u>.
7. Select the existing ItemType you wish to apply the rules to.
8. Select the stage definition created previously.
9. Select an Owner.
10. (Optional) Alter the default messages if you wish. Supported variables are:
   - {propLabel}
   - {propValue}
   - {itemKeyedName}
   - {itemTypeLabel}
   - {itemStateLabel}
11. Add the properties to be validated.
    1. Set expected values. (Default is *not null*.)
    2. Check if the property should be checked incoming from previous stages and stopped before getting to this stage if not valid.
    3. Set the stages to validate at (multi-select)
12. **Save** and **Lock** the record.
13. **Promote** the record to <u>Active</u>. (**If you do not do this, the rules will not be in force!**)
14. For the **ItemType(s)** selected above, go to **ItemTypes** and set <u>Server Events</u> for the following Method: pev_ItemProperty_extValidations to the following Events:
    1. OnAfterPromote
    2. OnAfterUpdate
    3. OnBeforePromote
15. Also, set the following <u>Action</u>: pav_ItemProperty_advValidations
16. Save/Unlock/Close the **ItemType**.
17. Test Validation.
    1. Checks will run on Promotions, and after the item is edited, before Save is applied.

## Contributing

1. Fork it!
2. Create your feature branch: `git checkout -b my-new-feature`
3. Commit your changes: `git commit -am 'Add some feature'`
4. Push to the branch: `git push origin my-new-feature`
5. Submit a pull request

For more information on contributing to this project, another Aras Labs project, or any Aras Community project, shoot us an email at araslabs@aras.com.

## Credits

Original Aras community project written by Rolf Laudenbach (rlaudenbach@aras.com) at Aras Corp.

## License

Aras Labs projects are published to GitHub under the MIT license. See the [LICENSE file](./LICENSE.md) for license rights and limitations.
