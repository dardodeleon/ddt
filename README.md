# ddt - Data-driven test

El objetivo de este repositorio es almacenar apuntes, que faciliten la creación de pruebas basadas en datos.

## Postman

### Test

```javascript
"use strict";

let ddt_id = "ddt_test";

let ddt_keys = null;

let ddt_tests = null;

let ddt_loaded = false;

pm.test('[0] Loading DDT', function() {

    pm.expect(pm.variables.has(ddt_id)).equal(true);
    
    ddt_tests = pm.variables.get(ddt_id);
    
    pm.expect(typeof ddt_tests).equal('object');
    
    ddt_keys = Object.keys(ddt_tests);
   
    pm.expect(ddt_keys.length).to.be.above(0);
    
    ddt_loaded = true;
});

if (ddt_loaded === true) {
    ddt_keys.forEach(function(ddt_name, ddt_id, _) {
        pm.test(`[${(1 + ddt_id)}] ${ddt_name}`, function () {
            const fn = new Function(['pm', 'console'], ddt_tests[ddt_name]);
            const result = fn.call(this, pm, console);
            if (result !== true) {
                pm.expect(result).equal(true);
            }
        });
    });
} else {
    postman.setNextRequest(null);
}
```

### Data + Tests

```json
[
  { 
    "key_1": 1, 
    "key_N": "value_N", 
    
    "ddt_test": {
        "Title test one": "JavaScript, pm... console...", 
        "Title test two": "..."
    }
  }
]
```
