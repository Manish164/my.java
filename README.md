import com.twilio.Twilio;
import com.twilio.base.ResourceSet;
import com.twilio.rest.whatsapp.v1.Account;
import com.twilio.rest.whatsapp.v1.PhoneNumber;
import com.twilio.rest.whatsapp.v1.message.MessageInstance;

import java.util.Date;

 class WhatsAppOnlineCheck {
    // Twilio API credentials
    public static final String ACCOUNT_SID = "AC38eef38dbf6ee6800543b7c13c213378";
    public static final String AUTH_TOKEN = "02e5b535b9a5b79bb87edadacb650b7c";
    // Phone number to check online status
    public static final String PHONE_NUMBER = "whatsapp:+919340039723";

    public static void main(String[] args) {
        // Initialize Twilio API client
        Twilio.init(ACCOUNT_SID, AUTH_TOKEN);
        // Get the account object
        Account account = Account.fetcher().fetch();
        // Check if the phone number is valid
        PhoneNumber phoneNumber = PhoneNumber.fetcher(PHONE_NUMBER).fetch();
        if (phoneNumber == null || !phoneNumber.isValid()) {
            System.out.println("Invalid phone number.");
            return;
        }
        // Query for the last seen status of the phone number
        ResourceSet<MessageInstance> messages = MessageInstance.reader(phoneNumber)
                .setPageSize(1).read();
        if (messages != null && messages.iterator().hasNext()) {
            MessageInstance message = messages.iterator().next();
            Date lastSeen = message.getDateSent();
            System.out.println("Last seen: " + lastSeen);
        } else {
            System.out.println("Phone number not found.");
        }
    }
}
