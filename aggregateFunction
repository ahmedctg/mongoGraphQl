### Mongodb Aggregate function for complex query...

## =========================================
db.invoices.aggregate([
         {
             $match: {
                 invoiceType:“invoice”,
                 tenantId: “sq9iavfoo24defpsi”
             }
         },
         {
             $project: {
                 invoiceOverDueTotal: {
                         $cond: { if: {
                         $eq: [“$status”,“overdue”]
                     }, then: “$invoiceTotal”, else: 0 }
                 },
                 invoiceDueTotal: {
                     $cond: { if: {
                         $not: [{
                         $in: [“$status”, [‘paid’, ‘lost’]]
                     }]
                     }, then: “$invoiceTotal”, else: 0 }
                 },
                 creditedAmount: {
                     $cond: { if: {
                        $not: [
                            {
                                $in: [“$status”, [‘paid’,‘lost’]]
                            }
                        ]
                     }, then: “$creditedAmount”, else: 0 }
                 },
                 totalPaid: {
                    $cond: { if: {
                        $not: [
                            {
                                $in: [“$status”, [‘paid’,‘lost’]]
                            }
                        ]
                     }, then: “$totalPaid”, else: 0 }
                 },
                 lostAmount: {
                     $cond: { if: { status:‘lost’ }, then: “$lostMeta.amount”, else:  0}
                 },
                status:1,
                lostMeta:1
             },
         },
         {
             $group: {
                 _id: null,
                invoiceOverDueTotal :{
                    $sum:‘$invoiceOverDueTotal’
                },
                invoiceDueTotal :{
                    $sum:‘$invoiceDueTotal’
                },
                creditedAmount :{
                    $sum:‘$creditedAmount’
                },
                totalPaid :{
                    $sum:‘$totalPaid’
                },
                lostAmount: {
                    $sum: ‘$lostAmount’
                }
             }
         },
         {
             $project: {
                 invoiceOverDueTotal:“$invoiceOverDueTotal”,
                 invoiceDueTotal:“$invoiceDueTotal”,
                 creditedAmount:“$creditedAmount”,
                 totalPaid:“$totalPaid”,
                 lostAmount:“$lostAmount”,
                 totalOverDue: {
                     $subtract: [{$add: [“$invoiceOverDueTotal”,“$creditedAmount”]},“$totalPaid”]
                 },
                 totalDue: {
                     $subtract: [{$add: [“$invoiceDueTotal”,“$creditedAmount”]},{$add: [“$totalPaid”,“$lostAmount”]}]
                 },
                 //dueTask: ‘$invoiceOverDueTotal’,
                 //invoiceThisMonth: ‘$invoiceOverDueTotal’,
             }
         }
     ])
