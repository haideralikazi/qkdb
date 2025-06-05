1. Table Structures (Updated)
1.1 Test Definitions Table (testDefs)
q
testDefs: ([] 
    // ===== CORE IDENTIFIERS =====
    testId: `symbol$();                // Unique test ID
    testName: `symbol$();              // Human-readable name
    
    // ===== STATIC CONNECTION INFO (Fallback) =====
    host: `symbol$();                  // Default host (if roster missing)
    port: `int$();                     // Default port (if roster missing)
    
    // ===== BUSINESS CONTEXT (Join Keys) =====
    zone: `symbol$();                  // e.g., `aws-us-east
    product: `symbol$();               // e.g., `marketData
    service: `symbol$();               // e.g., `pricingEngine
    resource: `symbol$();              // e.g., `tickPlant
    
    // ===== TEST CONFIGURATION =====
    testFunc: ();                      // Execution function
    description: ();                   // Detailed description
    daysOfWeek: ();                    // Days to run (1-7)
    runTimes: ();                      // Scheduled times
    runType: `symbol$();               // `times|`window|`count
    isActive: `boolean$();             // Enabled flag
    
    // ===== DYNAMIC LOOKUP CONTROL =====
    useRoster: `boolean$();            // Whether to use .dev.getRoster[]
    
    // ... (rest of columns remain same as previous version)
    // owner, createdBy, params, timeout, retryCount, etc.
)
1.2 Roster Table (Example Structure)
q
// Example roster table structure (returned by .dev.getRoster[])
// Columns must include at minimum:
rosterExample: ([]
    zone: `symbol$();
    product: `symbol$();
    service: `symbol$();
    resource: `symbol$();
    host: `symbol$();
    port: `int$()
)
2. Core Framework Updates
2.1 Dynamic Host/Port Resolution
q
// Get current connection details for a test
resolveEndpoint:{[test]
    $[not test`useRoster;
        // Use static values
        (test`host; test`port);
        
        // Dynamic lookup
        [roster:.dev.getRoster[];
         match:select from roster where
            zone=test`zone, product=test`product,
            service=test`service, resource=test`resource;
         
         $[0=count match;
            // Fallback to static if no roster entry
            (test`host; test`port);
            // Use roster values
            (first match`host; first match`port)
        ]
    ]
 }

// Enhanced test execution with dynamic endpoints
runTestWithRetry:{[testId;attempt]
    test: select from testDefs where testId=testId, isActive;
    if[0=count test; :(`status`errorMsg`attempt!(`failure;"Test not active";attempt))];
    test: first test;
    
    // Resolve host/port dynamically
    hostPort:resolveEndpoint[test];
    test[`host]:hostPort 0;
    test[`port]:hostPort 1;
    
    // ... rest of execution logic remains same ...
    startTime:.z.p;
    result:@[{(1b;value[x`testFunc];"")}[test]; {(0b;"";x)}; test`timeout];
    // ... (same as before)
 }
2.2 Example Roster Integration
q
// Mock .dev.getRoster function (replace with actual implementation)
.dev.getRoster:{[]
    (`zone`product`service`resource`host`port)!
    (`aws-us-east`aws-eu-west`azure-us; 
     `marketData`trading`risk;
     `pricing`matching`engine;
     `primary`secondary`backup;
     `md1`md2`trade1`trade2`risk1;
     5010 5011 5020 5021 5030)
 }

// Initialize with sample roster data
.mock.initRoster:{
    .dev.getRoster:{[]
        ([] 
            zone:`aws-us-east`aws-eu-west`aws-us-east;
            product:`marketData`trading`marketData;
            service:`pricing`matching`pricing;
            resource:`primary`primary`secondary;
            host:`md-prod1`trade-prod1`md-prod2;
            port:5010 5020 5011
        )
    };
    "Mock roster initialized"
 }
3. Full Framework Code
3.1 Initialization
q
initTables:{
    if[not `testDefs in tables`.; testDefs:: 0!testDefs];
    if[not `testResults in tables`.; testResults:: 0!testResults];
    
    // Example test with dynamic lookup
    if[0=count select from testDefs where testId=`dynamicExample;
        `testDefs insert (
            `dynamicExample; `DynamicEndpointTest; `fallbackHost; 5001;
            `aws-us-east; `marketData; `pricing; `primary;
            {[h;p] (h;p)"1+1"}; "Dynamic host/port test";
            1 2 3 4 5; 09:00:00; `times; 1b;
            1b;  // useRoster flag
            // ... other columns ...
        )
    ];
 }
3.2 Upsert Test (With Roster Support)
q
upsertTest:{[params]
    // Default useRoster to 0b if not specified
    defaults:`useRoster!(0b);
    params:defaults,params;
    
    // ... rest of upsert logic remains same ...
    // (from previous implementation)
 }
3.3 Query Helpers
q
// Find tests needing roster lookups
getDynamicTests:{[]
    select testId,testName,zone,product,service,resource,host,port 
    from testDefs 
    where useRoster
 }

// Check roster coverage
checkRosterCoverage:{[]
    tests:select testId,zone,product,service,resource from testDefs where useRoster;
    roster:.dev.getRoster[];
    aj[`zone`product`service`resource; tests; roster]
 }
4. Example Usage
4.1 Register Dynamic Test
q
upsertTest `testId`testName`zone`product`service`resource`useRoster`testFunc`description`isActive!
    (`priceCheck; `PriceSanity; `aws-us-east; `marketData; `pricing; `primary;
     1b; {[h;p] ...price validation logic... };
     "Validates pricing consistency"; 1b);
4.2 Runtime Behavior
When roster has entry:

q
// .dev.getRoster returns:
// zone       product    service  resource  host       port
// --------------------------------------------------------
// aws-us-east marketData pricing  primary   md-prod1   5010

runTest `priceCheck; // Uses md-prod1:5010
When roster missing entry:

q
// No matching roster entry -> falls back to testDefs defaults
runTest `priceCheck; // Uses fallbackHost:5001
4.3 Monitoring
q
// View dynamic tests and their resolved endpoints
select testId,host,port from testDefs where useRoster

// Check which tests lack roster coverage
select from checkRosterCoverage[] where null host
5. Key Features
Dynamic Resolution
Automatic Lookup: Tests with useRoster=1b check .dev.getRoster[] first

Graceful Fallback: Uses static host/port if roster lookup fails

Zero Config Changes: Test definitions remain valid during infrastructure changes

Operational Benefits
Infrastructure Agnostic: Tests don't require updates when hosts change

Environment Aware: Different endpoints per zone (prod vs. DR)

Audit Trail: testResults records actual host:port used

Maintenance Tools
q
// Update all dynamic tests to static
update useRoster:0b from `testDefs where useRoster;

// Bulk change roster usage by product
update useRoster:1b from `testDefs where product=`marketData;
6. Deployment Recommendation
Phase 1: Set useRoster:0b for all tests (static hosts)

Phase 2: Enable dynamic lookup for non-critical tests

Phase 3: Full rollout after roster validation

q
// Gradual rollout by severity
update useRoster:1b from `testDefs where severity=`low;
update useRoster:1b from `testDefs where severity=`medium;
update useRoster:1b from `testDefs where severity=`high;
This implementation provides maximum flexibility while maintaining backward compatibility with static host configurations.
