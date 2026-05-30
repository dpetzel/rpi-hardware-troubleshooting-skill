# Raspberry Pi 5 PMIC Breakdown
Very few resources seem to exist publicly documenting the POC and nearby components. This
is a breakdown of the PMIC area of the Pi 5 board. It represents the aggregation of my
own testing and information available on the Internet.

## Components

![Indexed Component List](./raspberrypi5-pmic_area_indexed.png)

<table>
  <tr>
    <th>#</th><th>Type</th><th>Package</th><th>Value</th><th>Power Rail</th><th>Links</th><th>Notes</th>
  </tr>
  <tr>
    <td>0</td>
    <td></td>
    <td>QFN</td>
    <td></td>
    <td>multiple</td>
    <td>
      <ul>
        <li><a href="https://thepihut.com/products/da9091-pmic-5-pack">Pi Hut Item</a></li>
      </ul>
    </td>
    <td>This is the PMIC itself. As of 2026-05-29 Pi Hut seems to be the only place to source one.</td>
  </tr>
  <tr><td>1</td></tr>
  <tr><td>2</td></tr>
  <tr><td>3</td></tr>
  <tr><td>4</td></tr>
  <tr><td>5</td></tr>
  <tr><td>6</td></tr>
  <tr><td>7</td>
    <td>Capacitor</td>
    <td>0402</td>
    <td></td>
    <td>5v</td>
    <td></td>
    <td>self-measured</td>
  </tr>
  <tr>
    <td>8</td>
    <td>Capacitor</td>
    <td>0402</td>
    <td>22µF</td>
    <td>5v</td>
    <td>
      <ul>
        <li><a href="https://www.digikey.com/short/d4jwf15w">DigiKey</a></li>
      </ul>
    </td>
    <td>Self measured</td>
  </tr>
  <tr><td>9</td>
    <td>Capacitor</td>
    <td>0402</td>
    <td></td>
    <td>5v</td>
    <td></td>
    <td>self-measured</td>
  </tr>
  <tr><td>10</td>
    <td>Capacitor</td>
    <td>0402</td>
    <td></td>
    <td>5v</td>
    <td></td>
    <td>self-measured</td>
  </tr>
  <tr><td>11</td></tr>
  <tr><td>12</td></tr>
  <tr><td>13</td></tr>
  <tr><td>14</td>
    <td>Capacitor</td>
    <td>0402</td>
    <td></td>
    <td>5v</td>
    <td></td>
    <td>self-measured</td>
  </tr>
  <tr><td>15</td></tr>
  <tr><td>16</td></tr>
  <tr><td>17</td></tr>
  <tr><td>18</td>
    <td>Capacitor</td>
    <td>0402</td>
    <td></td>
    <td>5v</td>
    <td></td>
    <td>self-measured</td>
  </tr>
  <tr><td>19</td></tr>
  <tr><td>20</td></tr>
  <tr><td>21</td></tr>
  <tr><td>22</td></tr>
  <tr><td>23</td></tr>
  <tr><td>24</td></tr>
  <tr><td>25</td></tr>
  <tr><td>26</td></tr>
  <tr><td>27</td></tr>
  <tr><td>28</td></tr>
  <tr><td>29</td>
    <td>Capacitor</td>
    <td>0402</td>
    <td></td>
    <td>5v</td>
    <td></td>
    <td>self-measured</td>
  </tr>
  <tr><td>30</td>
    <td>Capacitor</td>
    <td>0402</td>
    <td></td>
    <td>5v</td>
    <td></td>
    <td>self-measured</td>
  </tr>
  <tr><td>31</td></tr>
  <tr><td>32</td></tr>
  <tr><td>33</td></tr>
  <tr><td>34</td></tr>
  <tr><td>35</td>
    <td>Capacitor</td>
    <td>0402</td>
    <td></td>
    <td>5v</td>
    <td></td>
    <td>self-measured</td>
  </tr>
  <tr><td>36</td></tr>
  <tr><td>37</td></tr>
  <tr><td>38</td></tr>
  <tr><td>39</td></tr>
  <tr><td>40</td>
    <td>Capacitor</td>
    <td>0402</td>
    <td></td>
    <td>5v</td>
    <td></td>
    <td>self-measured</td>
  </tr>
  <tr><td>41</td>
    <td>Capacitor</td>
    <td>0402</td>
    <td></td>
    <td>5v</td>
    <td></td>
    <td>self-measured</td>
  </tr>
  <tr><td>42</td></tr>
  <tr><td>43</td></tr>
  <tr><td>44</td></tr>
</table>

