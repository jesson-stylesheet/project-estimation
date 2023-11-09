<script>

    let email = '';
    let projectType = 'new';
    let businessName = '';
    let currentWebsite = '';
    let budgetInMind = '';
    let budgetAmount = '';
    let pagesDesigning = 1;
    $: pagesDeveloping = pagesDesigning;
    let sitemapReady = '';
    let designVersion = '';
    let domainPurchased = '';
    let dnsHostingManagement = '';
    let dnsHosted = '';
    let cmsChoice = '';
    let hostingPreference = '';
    let needCopywriting = '';
    let seoContentNeeded = '';
    let contentProductionNeeded = '';
    let exampleWebsites = '';
    let basicSEO = '';
    let advancedSEO = '';
    let blogPagesMigrating = '';
    $: integrations = {
        gtm: { enabled: false, time: .5 },
        ga4: { enabled: false, time: .5 },
        gsc: { enabled: false, time: .5 },
        facebookPixel: { enabled: false, time: .75 },
        ecommerce: { enabled: false, time: 2 },
        socialMediaFeed: { enabled: false, time: 1 },
        googleMaps: { enabled: false, time: 1 },
        mailingCrmIntegration: { enabled: false, time: 2 },
        propertyListingCms: { enabled: false, time: 3 },
        chatBots: { enabled: false, time: 2 },
        bookingApps: { enabled: false, time: 1 },
        loyaltyReferralApps: { enabled: false, time: 1 },
        other: { enabled: false, time: 1.5 }
    };
    let productPages = '';
    let projectDeadline = '';
    let formSubmissionEmail = '';
    let spamFilterSetup = '';
    let websiteTrainingNeeded = '';
    let additionalInfo = '';

    let integrationTime = 0; // This will hold the total time from all checked integrations
    let timeEstimate = 0;
    let costEstimate = 0;

    let seoEnabled = false; // Initial value
    let shopifyChosen = false; // Initial value

    let showWarning = false; // Reactive statement to show warning
    $: showWarning = pagesDeveloping < pagesDesigning;
    $: warningPanel = pagesDeveloping < pagesDesigning ? 'warningPanel' : '';

    // Reactive statements
    $: seoEnabled = basicSEO === 'yes' || advancedSEO === 'yes';
    $: shopifyChosen = cmsChoice === 'shopify';

    // Reactive statement to update integrations when seoEnabled changes
    $: if (seoEnabled) {
        let updatedIntegrations = { ...integrations };
        updatedIntegrations.gtm.enabled = seoEnabled;
        updatedIntegrations.ga4.enabled = seoEnabled;
        updatedIntegrations.gsc.enabled = seoEnabled;
        
        integrations = updatedIntegrations; // Reassign to trigger reactivity
    }

    // Reactive statement to update integrations when shopifyChosen changes
    $: if (shopifyChosen ) {
        let updatedIntegrations = { ...integrations };
        updatedIntegrations.ecommerce.enabled = shopifyChosen;
        
        integrations = updatedIntegrations; // Reassign to trigger reactivity
    }

    function calculateIntegrationTime() {
        return Object.values(integrations).reduce((totalTime, integration) => {
            return integration.enabled ? totalTime + integration.time : totalTime;
        }, 0);
    }

    // Reactive statement to calculate integration time
    $: if(integrations) {integrationTime = calculateIntegrationTime();}

    // Reactive statement to recalculate estimations whenever the inputs change
    $: {
		let npd = Number(pagesDesigning); // number of pages being designed
		let npv = Number(pagesDeveloping); //number of pages being developed
        let baseCost = 0;

        timeEstimate = 0;

        // Add 1 hour if there's no existing sitemap
        if (sitemapReady === 'no') {
            timeEstimate += 1;
        } else if (sitemapReady === 'maybe') {
			timeEstimate += 0.5;
		}

        // Multiply by 2 if designing for both desktop and mobile
        if (designVersion === "both") {
            npd *= 2;
        }

        // Add 1 hour if the client has not bought a domain, add 0.5 hour if 'Maybe'
        if (domainPurchased === 'no') {
            timeEstimate += 1;
        } else if (domainPurchased === 'maybe') {
            timeEstimate += 0.5;
        }

        // Add 1 hour if managing the DNS for the client
        if (dnsHostingManagement === 'us') {
            timeEstimate += 1;
        }

        // Multiply by 1.5 if the CMS of choice is Webflow, WordPress, Framer, HubSpot or Other
        if (['webflow', 'wordpress', 'framer', 'hubspot', 'other'].includes(cmsChoice)) {
            npv *= 1.5;
        }

		// Copywriting content needed for the website
		if (needCopywriting === 'yes') {
            npv *= 1.25;
        }

		// Website's copy need to be SEO
		if (seoContentNeeded === 'yes') {
			npv *= 1.15;
		}

		// Website's need content to be produced
		if (contentProductionNeeded === 'maybe') {
			npv *= 1.15;
		}else if (contentProductionNeeded === 'yes') {
            npv *= 1.25;
        }

		// Website's need content to be produced
		if (basicSEO === 'yes') {
			npv *= 1.15;
		}

		// Website's need content to be produced
		if (advancedSEO === 'yes') {
			npv *= 1.5;
		}

		if (spamFilterSetup === 'yes') {
			timeEstimate += 0.5;
		}

		if (websiteTrainingNeeded === 'yes') {
			timeEstimate += 1;
		}

		timeEstimate += Number(blogPagesMigrating)*0.25;
        timeEstimate += Number(productPages)*0.25;

        if (npv > 10) {
            timeEstimate += npd * 1.15;
        }

		// Add 1 hour for every page we design
		timeEstimate += npd*1.5;
        
        timeEstimate += integrationTime;

		// Add 1 hour for every page we develop
		timeEstimate += npv-npd;

        if (npv !== npd && npv/npd < 9.9) {
            timeEstimate += 1;
        }

		// New or revamp website project
		if (projectType === 'revamp') {
			timeEstimate *= 0.75;
		}

        if (timeEstimate > 0) {
            baseCost = 760;
        }

		// Rounding to 2 decimal places
		timeEstimate = Number(timeEstimate.toFixed(2));
        // Assuming $160 per hour for cost estimate
        costEstimate = (timeEstimate * 160) + baseCost;
		costEstimate = Number(costEstimate.toFixed(2));
    }
    let isSubmitted = false;
    function handleSubmit() {
    // Here you would typically handle the form submission,
    // like sending data to a server. For this example,
    // we'll just set isSubmitted to true to show the message.
    isSubmitted = true;
    }
</script>

<style>
	.body {
		display: block;
		margin: auto;
		max-width: 1080px;
	}

    h1 {
        margin-bottom: 0;
    }

    .desc {
        margin-top: 8px;
        margin-bottom: 32px;
        max-width: 70%;
    }
    .form-container {
        display: flex;
        justify-content: space-between;
    }

    .form-content {
        /* Adjust the width as needed */
        width: 70%;
    }

    .estimation-panel {
        position: sticky;
        top: 20px; /* Adjust the top position as needed */
        width: 33%; /* Adjust the width as needed */
		min-width: 200px;
		height: 488px;
        padding: 10px;
        margin-left: 5%;
        background: #f9f9f9;
        border: 1px solid #ddd;
        border-radius: 4px;
        box-shadow: 0 2px 4px rgba(0,0,0,0.1);
        /* This will ensure the container doesn't overlap with the footer or header */
        z-index: 10;
    }

	.integration-item {
    display: flex;
    align-items: center;
    margin-bottom: 8px; /* Adjust the space between each checkbox as needed */
  	}

  	.integration-item input[type="checkbox"] {
    margin-right: 5px; /* Adjust the space between the checkbox and the label as needed */
  	}

  	.integration-item label {
    margin: 0; /* Removes default margin from labels */
  	}

    #warning {
        color: red;
    }

    .thankyou {
        display: inline-block;
        font-weight: 800;
        animation: waviy 1s infinite;
        animation-delay: calc(.1s * var(--i));
    }
    @keyframes waviy {
        0%,40%,100% {
            transform: translateY(0)
        }
        20% {
            transform: translateY(-16px)
        }
    }

    @media (max-width: 760px) {
        .desc {
        max-width: 100%;
        }
        .estimation-panel {
            margin-left: 2%;
        }
    }

	@media (max-width: 660px) {
    	.estimation-panel {
        	position: fixed; /* Fixed positioning to stick the panel at the bottom right corner */
        	top: 80vh;
        	right: 0; /* Align to the right */
			height: 120px;
        	width: 20%; /* Optional: make the panel full width on smaller screens */
		}
		.form-content {
        	/* Adjust the width as needed */
        	width: 100%;
    	}
        #warningPanel {
            top: 70vh !important;
            height: 200px !important;
        }
    }

    @media (max-width: 444px) {
        .body::after {
            content: " ";
            display: block;
            height: 100px;
        }
    }
</style>
<div class="body">
<h1>Website Project Checklist</h1>
<p class="desc">This checklist is intended to guide the website project in the scoping phase and to gather necessary information from the client before the project begins.</p>
<div class="form-container">
    {#if isSubmitted}
    <div style="display:inline-block;">
  <span class="thankyou" style="--i:1">Thank </span>
  <span class="thankyou" style="--i:2">you </span>
  <span class="thankyou" style="--i:3">for </span>
  <span class="thankyou" style="--i:4">submitting </span>
  <span class="thankyou" style="--i:5">the </span>
  <span class="thankyou" style="--i:6">form! </span>
</div>
{:else}
    <div class="form-content">
<form on:submit|preventDefault={handleSubmit}>
    <!-- Email -->
    <div>
        <label for="email">Your Email:</label>
        <input type="email" bind:value={email} required>
    </div>

    <!-- Project Type -->
    <div>
        <label for="projectType"> Would this be a new website project or a revamp of a current website?</label>
            <input type="radio" bind:group={projectType} value="new" required> New
			<br>
            <input type="radio" bind:group={projectType} value="revamp"> Revamp
    </div>

    <!-- Client's Business Name -->
    <div>
        <label for="businessName">What is the client's business name?</label>
        <input type="text" bind:value={businessName} required>
    </div>

    <!-- Current Website -->
    <div>
        <label for="currentWebsite">What is their current website (if applicable)?</label>
        <input type="text" bind:value={currentWebsite}>
    </div>

    <!-- Budget In Mind -->
    <div>
        <label>Does the client have a budget in mind?</label>
        <select bind:value={budgetInMind}>
            <option value="">Select an option</option>
            <option value="yes">Yes</option>
            <option value="no">No</option>
            <option value="maybe">Maybe</option>
        </select>
    </div>

    <!-- Budget Amount -->
    <div>
        <label for="budgetAmount">How much (if applicable)?</label>
        <input type="number" bind:value={budgetAmount}>
    </div>

<hr><br>
    <!-- Pages Designing -->
    <div>
        <label for="pagesDesigning">How many pages are we designing/templating?</label>
        <input type="number" bind:value={pagesDesigning}>
    </div>

    <!-- Pages Developing -->
    <div>
        <label for="pagesDeveloping">How many pages are we developing? (excld. blog & product pages)</label>
        <input type="number" bind:value={pagesDeveloping}>
    </div>

    <!-- Sitemap Ready -->
    <div>
        <label>Do we have a sitemap ready?</label>
        <select bind:value={sitemapReady}>
            <option value="">Select an option</option>
            <option value="yes">Yes</option>
            <option value="no">No</option>
            <option value="maybe">Maybe</option>
        </select>
    </div>

    <!-- Design Version -->
    <div>
        <label>Are we designing the desktop version only, mobile version only, or both?</label>
        <select bind:value={designVersion}>
            <option value="">Select an option</option>
            <option value="desktop">Desktop Only</option>
            <option value="mobile">Mobile Only</option>
            <option value="both">Both</option>
        </select>
    </div>

    <!-- Domain Purchased -->
    <div>
        <label>Has the client bought a domain name yet?</label>
        <select bind:value={domainPurchased}>
            <option value="">Select an option</option>
            <option value="yes">Yes</option>
            <option value="no">No</option>
            <option value="maybe">Maybe</option>
        </select>
    </div>

    <!-- DNS and Hosting Management -->
    <div>
        <label>Will the client be managing the DNS and hosting themselves, or are we contracted to do it?</label>
        <select bind:value={dnsHostingManagement}>
            <option value="">Select an option</option>
            <option value="client">The Client</option>
            <option value="us">We'll be doing it</option>
        </select>
    </div>

    <!-- DNS Hosted -->
    <div>
        <label>Where is the DNS hosted?</label>
        <select bind:value={dnsHosted}>
            <option value="">Select an option</option>
            <option value="crazydomains">Crazy Domains</option>
            <option value="ventraip">VentraIP</option>
            <option value="godaddy">GoDaddy</option>
            <option value="namecheap">Namecheap</option>
            <option value="google">Google Domains</option>
            <option value="cloudflare">Cloudflare</option>
            <option value="other">Other</option>
        </select>
    </div>

    <!-- CMS Choice -->
    <div>
        <label>Which CMS are we using for the website?</label>
        <select bind:value={cmsChoice}>
            <option value="">Select an option</option>
            <option value="wordpress">WordPress</option>
            <option value="webflow">Webflow</option>
            <option value="wix">Wix</option>
            <option value="shopify">Shopify</option>
            <option value="squarespace">Squarespace</option>
            <option value="framer">Framer</option>
            <option value="hubspot">HubSpot</option>
            <option value="other">Other</option>
        </select>
    </div>

    <!-- Hosting Preference -->
    <div>
        <label>Where does the client want the website to be hosted?</label>
        <select bind:value={hostingPreference}>
            <option value="">Select an option</option>
            <option value="cms_hosted">Hosted by the CMS (e.g., Wix, Webflow)</option>
            <option value="third_party_host">3rd Party Host (e.g., VentraIP, Crazy Domains)</option>
            <option value="own_server">Own Server</option>
        </select>
    </div>
	
	<hr><br>
    <!-- Copywriting Content Needed -->
    <div>
        <label>Does the client need copywriting content?</label>
        <select bind:value={needCopywriting}>
            <option value="">Select an option</option>
            <option value="yes">Yes</option>
            <option value="no">No</option>
            <option value="maybe">Maybe</option>
        </select>
    </div>

    <!-- SEO Content Needed -->
    <div>
        <label>Has the client asked for SEO content?</label>
        <select bind:value={seoContentNeeded}>
            <option value="">Select an option</option>
            <option value="yes">Yes</option>
            <option value="no">No</option>
            <option value="maybe">Maybe</option>
        </select>
    </div>

    <!-- Content Production Needed -->
    <div>
        <label>Is content production needed?</label>
        <select bind:value={contentProductionNeeded}>
            <option value="">Select an option</option>
            <option value="yes">Yes</option>
            <option value="no">No</option>
            <option value="maybe">Maybe</option>
        </select>
    </div>

    <!-- Basic SEO -->
    <div>
        <label>Is basic SEO setup required?</label>
        <select bind:value={basicSEO}>
            <option value="">Select an option</option>
            <option value="yes">Yes</option>
            <option value="no">No</option>
            <option value="maybe">Maybe</option>
        </select>
    </div>

    <!-- Advanced SEO -->
    <div>
        <label>Is advanced SEO setup required?</label>
        <select bind:value={advancedSEO}>
            <option value="">Select an option</option>
            <option value="yes">Yes</option>
            <option value="no">No</option>
            <option value="maybe">Maybe</option>
        </select>
    </div>

    <!-- Blog Pages Migrating -->
    <div>
        <label for="blogPagesMigrating">How many blog pages are we migrating?</label>
        <input type="number" bind:value={blogPagesMigrating}>
    </div>

    <!-- Integrations Needed -->
	<div>
        <label>What integrations are needed?</label>
        <div class="integration-item">
          <input type="checkbox" id="gtm" bind:checked={integrations.gtm.enabled}>
          <label for="gtm">Google Tag Manager (GTM)</label>
        </div>
        <div class="integration-item">
          <input type="checkbox" id="ga4" bind:checked={integrations.ga4.enabled}>
          <label for="ga4">Google Analytics 4 (GA4)</label>
        </div>
        <div class="integration-item">
          <input type="checkbox" id="gsc" bind:checked={integrations.gsc.enabled}>
          <label for="gsc">Google Search Console (GSC)</label>
        </div>
        <div class="integration-item">
          <input type="checkbox" id="facebook-pixel" bind:checked={integrations.facebookPixel.enabled}>
          <label for="facebook-pixel">Facebook Pixel</label>
        </div>
        <div class="integration-item">
          <input type="checkbox" id="ecommerce" bind:checked={integrations.ecommerce.enabled}>
          <label for="ecommerce">E-commerce</label>
        </div>
        <div class="integration-item">
          <input type="checkbox" id="social-media-feed" bind:checked={integrations.socialMediaFeed.enabled}>
          <label for="social-media-feed">Social Media Feed</label>
        </div>
        <div class="integration-item">
          <input type="checkbox" id="google-maps" bind:checked={integrations.googleMaps.enabled}>
          <label for="google-maps">Google Maps</label>
        </div>
        <div class="integration-item">
          <input type="checkbox" id="mailing-crm-integration" bind:checked={integrations.mailingCrmIntegration.enabled}>
          <label for="mailing-crm-integration">Mailing/CRM Integration (e.g., Mailchimp)</label>
        </div>
        <div class="integration-item">
          <input type="checkbox" id="property-listing-cms" bind:checked={integrations.propertyListingCms.enabled}>
          <label for="property-listing-cms">Property Listing CMS</label>
        </div>
        <div class="integration-item">
          <input type="checkbox" id="chat-bots" bind:checked={integrations.chatBots.enabled}>
          <label for="chat-bots">Chat Bots</label>
        </div>
        <div class="integration-item">
          <input type="checkbox" id="booking-apps" bind:checked={integrations.bookingApps.enabled}>
          <label for="booking-apps">Booking Apps</label>
        </div>
        <div class="integration-item">
          <input type="checkbox" id="loyalty-referral-apps" bind:checked={integrations.loyaltyReferralApps.enabled}>
          <label for="loyalty-referral-apps">Loyalty/Referral Apps</label>
        </div>
        <div class="integration-item">
          <input type="checkbox" id="other-integrations" bind:checked={integrations.other.enabled}>
          <label for="other-integrations">Other</label>
        </div>
      </div>
      
      {#if integrations.ecommerce.enabled}
        <div>
            <label for="productPages">How many product pages are we creating?</label>
            <input type="number" bind:value={productPages}>
        </div>
        {/if}

	<hr><br>
    <!-- Project Deadline -->
    <div>
        <label for="projectDeadline">When is the project deadline?</label>
        <input type="date" bind:value={projectDeadline}>
    </div>

    <!-- Example Websites -->
    <div>
        <label for="exampleWebsites">Please list any example websites the client likes:</label>
        <textarea bind:value={exampleWebsites}></textarea>
    </div>

    <!-- Form Submission Email -->
    <div>
        <label for="formSubmissionEmail">Where should the form submissions go?</label>
        <input type="email" bind:value={formSubmissionEmail}>
    </div>

    <!-- Spam Filter Setup -->
    <div>
        <label>Is a spam filter setup required?</label>
        <select bind:value={spamFilterSetup}>
            <option value="">Select an option</option>
            <option value="yes">Yes</option>
            <option value="no">No</option>
            <option value="maybe">Maybe</option>
        </select>
    </div>

    <!-- Website Training Needed -->
    <div>
        <label>Is website training needed for the client?</label>
        <select bind:value={websiteTrainingNeeded}>
            <option value="">Select an option</option>
            <option value="yes">Yes</option>
            <option value="no">No</option>
            <option value="maybe">Maybe</option>
        </select>
    </div>

    <!-- Additional Information -->
    <div>
        <label for="additionalInfo">Any additional information:</label>
        <textarea bind:value={additionalInfo}></textarea>
    </div>

    <!-- Submission -->
    <div>
        <button type="submit">Submit</button>
    </div>
</form>
</div>
{/if}
<!-- Panel to display time and cost estimation -->
<div class="estimation-panel" id={warningPanel}>
    <p>Time Estimate: <span>{timeEstimate}</span> hours</p>

    <p>Cost Estimate: <span>${costEstimate}</span></p>

    {#if showWarning}
        <p id="warning">WARNING: The number of pages being developed should not be less than the number of pages being designed.</p>
    {/if}
</div>
</div>
</div>