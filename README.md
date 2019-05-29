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

  > Note: The word <u>advanced</u> has been used in this file to describe the properties as opposed to <u>extended</u> since this is close to the description of xProperties in the Aras system. However, all parts of the package and documentation could not accommodate this update.

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

### Post Installation Steps - Advanced Property Validations

- [ ] Log in as <u>admin</u> and expand to **Administration > Configuration**
- [ ] In category **ItemType Stages Definition**:  add stages definitions for ItemType to add advanced property validations to.
- [ ] In category **Property Validation Rules**:  add rules definitions for ItemType to add advanced property validations to (connected to ItemType Stages).
    - [ ] Promote rules to <u>Active</u> (**If you skip this step the rules will not be in force!**)

- [ ] For ItemType to add advanced Property validations to add method "pev_ItemProperty_extValidations" to Server Event:  "onAfterUpdate"
    - [ ] For ItemType to add advanced Property validations to add action "pev_ItemProperty_extValidations"
    - [ ] For ItemType to add advanced Property validations to add method "pev_ItemProperty_extValidations" to Server Event:  "onBeforePromote"  AND "onAfterPromote", if validation shall be done on life cycle promote, as well!

## Usage

1. Open **ItemType Stages Definition**. Create a <u>New Item</u>.
2. Select the existing ItemType you wish to apply the rules to.
3. For Each Definition:
   1. Select the Itemtype you wish to pull the LC states from. 
   2. Map existing Lifecycle states to your custom stage names (Multi-lingual capable). This is a Many to One mapping.
   3. (Optional) Previous stages are used if you wish to define what the incoming state should be for the check. [For instance, if you have two branches coming into a state and you only want the validation to run if it came from Branch B and not Branch A.]
4. Save/Unlock/Close the record.
5. Open  **Property Validation Rules**. Create a <u>New Item</u>.
6. Select the existing ItemType you wish to apply the rules to.
7. Select the stage definition created previously.
8. Select an Owner.
9. (Optional) Alter the default messages if you wish. Supported variables are:
   - {propLabel}
   - {propValue}
   - {itemKeyedName}
   - {itemTypeLabel}
   - {itemStateLabel}
10. Add the properties to be validated.
    1. Set expected values. (Default is *not null*.)
    2. Check if the property should be checked incoming from previous stages and stopped before getting to this stage if not valid.
    3. Set the stages to validate at (multi-select)
11. **Save** and **Lock** the record.
12. **Promote** the record to <u>Active</u>. (**If you do not do this, the rules will not be in force!**)
13. For the **ItemType(s)** selected above, go to **ItemTypes** and set <u>Server Events</u> for the following Method: pev_ItemProperty_extValidations to the following Events:
    1. OnAfterPromote
    2. OnAfterUpdate
    3. OnBeforePromote
14. Also, set the following <u>Action</u>: pev_ItemProperty_extValidations
15. Save/Unlock/Close the **ItemType**.
16. Test Validation.
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