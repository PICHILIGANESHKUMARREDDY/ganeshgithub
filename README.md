# ganeshgithub
import com.sap.gateway.ip.core.customdev.util.Message
import java.util.HashMap
import groovy.xml.XmlSlurper

def Message processData(Message message) {
    def body = message.getBody(java.lang.String) as String
    def xml = new XmlSlurper().parseText(body)

    xml.Order.each { order ->
        def orderId = order.OrderID.text()
        def customer = order.CustomerName.text()
        def amount = order.Amount.text()

        messageLogFactory.getMessageLog(message).addAttachment("Order Info", 
            "OrderID: ${orderId}, Customer: ${customer}, Amount: ${amount}", "text/plain")
    }

    return message
}
