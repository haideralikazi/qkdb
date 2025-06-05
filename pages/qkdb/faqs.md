1.1 Test Definitions Table (testDefs)

testDefs: ([] 
    // ===== CORE TEST METADATA =====
    testId: `symbol$();                // Unique identifier (e.g., `tradeVolCheck)
    testName: `symbol$();              // Human-readable name (e.g., `TradeVolumeAlert)
    host: `symbol$();                  // Target host (e.g., `prod-md1)
    port: `int$();                     // Target port (e.g., 5010)
    testFunc: ();                      // Function to execute
    
    // ===== BUSINESS CONTEXT =====
    zone: `symbol$();                  // Infrastructure zone (e.g., `aws-us-east)
    product: `symbol$();               // Product line (e.g., `marketData)
    service: `symbol$();               // Service name (e.g., `pricingEngine)
    resource: `symbol$();              // Resource type (e.g., `tickPlant)
    
    // ===== OWNERSHIP =====
    owner: `symbol$();                 // Team/owner (e.g., `quantTeam)
    createdBy: `symbol$();             // Creator username (e.g., `jane.doe)
    
    // ===== TEST CONFIGURATION =====
    description: ();                   // Detailed description
    daysOfWeek: ();                    // Days to run (1-7, Monday=1)
    runTimes: ();                      // Scheduled times (HH:MM:SS)
    runType: `symbol$();               // `times|`window|`count
    isActive: `boolean$();             // Enabled flag
    
    // ===== EXECUTION CONTROL =====
    params: ();                        // Key-value parameters
    timeout: `timespan$();             // Max runtime (e.g., 0D00:00:30)
    retryCount: `int$();               // Retry attempts
    retryDelay: `timespan$();          // Delay between retries
    
    // ===== GROUPING =====
    groupName: `symbol$();             // Logical group (e.g., `eodChecks)
    groupDescription: ();              // Group purpose
    groupSchedule: ();                 // Override schedule
    runInGroupOnly: `boolean$();       // Group-only execution
    
    // ===== DEPENDENCIES =====
    dependsOn: ();                     // Required passing tests
    
    // ===== STATUS TRACKING =====
    created: `timestamp$();            // Creation time
    modified: `timestamp$();           // Last modified
    lastRun: `timestamp$();            // Last execution
    lastStatus: `symbol$();            // `success|`failure|`timeout
    lastError: ();                     // Last error message
    lastOutput: ();                    // Last raw output
    
    // ===== ALERTING =====
    notifyEmail: ();                   // Recipient emails
    severity: `symbol$()               // `high|`medium|`low
)


