# atom-holochain-snippets

Atom package for Holochain snippets (hdk-rust)

[Install](https://atom.io/packages/atom-holochain-snippets)

Snippets:

| snippet | result |
| ---     | ---    |
| hclib | Default crate imports for main lib.rs file |
| hcmod | Default crate imports for modules where you define application logic |
| hczome | define_zome! macro |
| hcdef | definition() |
| hcentry | entry! macro |
| hcfun | zome function |
| hclink | link! macro |
| hcfrom | from! macro |
| hcto | to! macro |
| hcstruct | new entry struct/impl |
| hccreate | basic create entry function |
| hcget | basic get entry function |
| hcupdate | basic update entry function |
| hcdelete | basic delete entry function |
| hcsend | basic send payload function |

<hr>

### hclib 
Default crate imports for main lib.rs file
```rust
#![feature(try_from)]
#[macro_use]
extern crate hdk;
extern crate serde;
#[macro_use]
extern crate serde_derive;
extern crate serde_json;
#[macro_use]
extern crate holochain_core_types_derive;

use hdk::{
    entry_definition::ValidatingEntryType,
    error::ZomeApiResult,
};

use hdk::holochain_core_types::{
    cas::content::Address,
    entry::Entry,
    dna::entry_types::Sharing,
    error::HolochainError,
    json::JsonString,
    validation::EntryValidationData
};
```

### hcmod 
Default crate imports for modules where you define application logic
```rust
use hdk::{
    self,
    error::{ZomeApiError, ZomeApiResult},
    holochain_core_types::{
        cas::content::Address,
        dna::capabilities::CapabilityRequest,
        entry::{cap_entries::CapabilityType, entry_type::EntryType, Entry},
        error::HolochainError,
        json::JsonString,
        signature::{Provenance, Signature},
    },
    holochain_wasm_utils::api_serialization::{
        commit_entry::CommitEntryOptions,
        get_entry::{
            EntryHistory, GetEntryOptions, GetEntryResult, GetEntryResultType, StatusRequestKind,
        },
        get_links::{GetLinksOptions, GetLinksResult},
        QueryArgsOptions, QueryResult,
    },
    AGENT_ADDRESS, AGENT_ID_STR, CAPABILITY_REQ, DNA_ADDRESS, DNA_NAME, PROPERTIES, PUBLIC_TOKEN,
};
```

### hczome 
define_zome! macro
```rust
define_zome! {
    entries: [
        $1
    ]
    genesis: || {
        Ok(())
    }
    functions: [
    
    ]
    traits: {
        hc_public [
        
        ]
    }
}
```

### hcdef 
definition()
```rust
pub fn definition() -> ValidatingEntryType {
    $1
}
```

### hcentry 
entry! macro
```rust
entry!(
    name: ${1:"myentry"},
    description: ${2:"description"},
    sharing: Sharing::${3:Public},
    validation_package: || {
        hdk::ValidationPackageDefinition::${4:Entry}
    },
    validation: |_validation_data: ValidationData<${5:EntryType}>| {
        ${6:Ok(())}
    },
    links: [
        $7
    ]
)
```

### hcfun 
zome function
```rust
${1:my_function_name}: {
    inputs: | $2 |,
    outputs: | $3 |,
    handler: handle_${1:my_function_name}
}
```

### hclink 
link! macro
```rust
link!(
    hdk::LinkDirection::${1:From},
    ${2:"%agent_id"},
    link_type: ${3:"my_link_type"},
    validation_package: || {
        hdk::ValidationPackageDefinition::${4:ChainFull}
    },
    validation: | _validation_data: hdk::LinkValidationData | {
        ${5:Ok(())}
    }
)
```

### hcfrom 
from! macro
```rust
from!(
    ${1:"%agent_id"},
    link_type: ${2:"my_link_type"},
    validation_package: || {
        hdk::ValidationPackageDefinition::${3:ChainFull}
    },
    validation: | _validation_data: hdk::LinkValidationData | {
        ${4:Ok(())}
    }
)
```

### hcto 
to! macro
```rust
to!(
    ${1:"entry_name"},
    link_type: ${2:"my_link_type"},
    validation_package: || {
        hdk::ValidationPackageDefinition::${3:ChainFull}
    },
    validation: | _validation_data: hdk::LinkValidationData | {
        ${4:Ok(())}
    }
)
```

### hcstruct 
new entry struct + impl
```rust
#[derive(Serialize, Deserialize, Debug, DefaultJson)]
struct ${1:MyEntryType} {
    ${2:my_field}: String
}
impl ${1:MyEntryType} {
    pub fn new(${2:my_field}: &str) -> ${1:MyEntryType} {
        ${1:MyEntryType} {
            ${2:my_field}: ${2:my_field}.to_owned()
        }
    }
    pub fn ${2:my_field}(&self) -> String {
        self.${2:my_field}.clone()
    }
}
```

### hccreate 
basic create entry function
```rust
fn handle_create_${1:my_entry}(${2:my_field}: String) -> ZomeApiResult<Address> {
    let address = hdk::commit_entry(Entry::App("${1:my_entry}".into(), ${3:MyEntryType}::new(&${2:my_field}, "now").into()))?;
    Ok(address)
}
```

### hcget 
basic get entry function
```rust
fn handle_get_${1:my_entry}(${1:my_entry}_address: Address) -> ZomeApiResult<Option<Entry>> {
    hdk::get_entry(&${1:my_entry}_address)
}
```

### hcupdate 
basic update entry function
```rust
fn handle_update_${1:my_entry}(${1:my_entry}_address: Address, new_content: ${3:String}) -> ZomeApiResult<Address> {
    let updated_${1:my_entry} = Entry::App(
        "${1:my_entry}".into(),
        ${2:MyEntryType}::new(&new_content).into(),
    );
    hdk::update_entry(updated_${1:my_entry}, &${1:my_entry}_address)
}
```

### hcdelete 
basic delete entry function
```rust
fn handle_delete_${1:my_entry}(${1:my_entry}_address: Address) -> ZomeApiResult<Address> {
    hdk::get_entry(&${1:my_entry}_address)?;
    hdk::remove_entry(&${1:my_entry}_address)
}
```

### hcsend 
basic send payload function
```rust
fn handle_send_${1:my_payload}(to_agent: Address, payload: ${2:String}) -> ZomeApiResult<String> {
    hdk::send(to_agent, payload, ${3:60000}.into())
}
```
