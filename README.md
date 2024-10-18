# Chatbot Integration Script

This script integrates a chatbot onto a webpage using the Sendbird API and customizes its behavior. It hides specific UI elements and changes the placeholder text to provide a better user experience.

## Overview

- **Purpose**: Embeds a chatbot into a webpage, hides certain elements, and customizes the placeholder text.
- **Dependencies**: Uses the Sendbird Chatbot API, loaded from `https://aichatbot.sendbird.com/index.js`.
- **Customization**: Applies custom CSS for element visibility and JavaScript to modify placeholder text.

## Script Explanation

```html
<script>
    !function(w, d, s, ...args) {
        // Create a style element to hide the specific div
        var style = d.createElement('style');
        style.innerHTML = `
            .sc-jaXxmE.heKXtN {
                display: none !important;
            }
        `;
        d.head.appendChild(style); // Append the style to the document head

        // Create the chatbot div
        var div = d.createElement('div');
        div.id = 'aichatbot';
        d.body.appendChild(div);

        // Store chatbot configuration
        w.chatbotConfig = args;
        var f = d.getElementsByTagName(s)[0],
            j = d.createElement(s);
        j.defer = true;
        j.type = 'module';
        j.src = 'https://aichatbot.sendbird.com/index.js';
        f.parentNode.insertBefore(j, f);

        // Function to change placeholder text
        function changePlaceholder() {
            var placeholder = d.querySelector('.sendbird-message-input--placeholder');
            if (placeholder) {
                placeholder.textContent = '💡 Aide-toi, pose une question à TutoBot ! 😊';
            }
        }

        // Change placeholder text after chatbot is loaded
        j.onload = function() {
            // Initial change
            changePlaceholder();

            // Create a MutationObserver to watch for changes in the DOM
            var observer = new MutationObserver(function(mutations) {
                mutations.forEach(function(mutation) {
                    // Check if the mutation added new nodes
                    if (mutation.addedNodes.length) {
                        changePlaceholder(); // Change placeholder if new nodes are added

                        // Reset placeholder if it changes back to original
                        var placeholder = d.querySelector('.sendbird-message-input--placeholder');
                        if (placeholder && placeholder.textContent === 'Enter message') {
                            changePlaceholder(); // Reset placeholder
                        }
                    }
                });
            });

            // Start observing the document body for child node changes
            observer.observe(d.body, { childList: true, subtree: true });
        };
    }(window, document, 'script', '', 'onboarding_bot', {
        apiHost: 'https://api-cf-eu-1.sendbird.com',
    });
</script>
